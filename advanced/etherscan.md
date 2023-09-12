---
order: -1
---

# Verify on Etherscan

To verify your contracts on Etherscan you can use [post-deploy hooks](../configuration/hooks.md).

We first need a script to perform the verification. Our requirements:

* Load deployment information from the [deployment records](../commands/deploy.md).
* Load deployment network information from the [hook environment](../configuration/hooks.md).
* Only verify for public chains, and not local test nodes.

Here is the [Foundry example project script](https://github.com/gemstation/contracts-foundry/blob/master/scripts/verify.js):

```js
#!/usr/bin/env node

// file: scripts/verify.js

(async () => {
  require('dotenv').config()
  const { $ } = (await import('execa'))

  const addresses = require('../gemforge.deployments.json')

  // this env var gets set automatically by Gemforge during the deployment process
  const chainId = process.env.GEMFORGE_DEPLOY_CHAIN_ID
  if (!chainId) {
    throw new Error('GEMFORGE_DEPLOY_CHAIN_ID env var not set')
  }

  // skip localhost
  if (chainId === '31337') {
    console.log('Skipping verification on localhost')
    return
  }

  console.log(`Verifying on chain ${chainId} ...`)

  const contracts = addresses[chainId] || []

  for (let { name, contract } of contracts) {
    let args = '0x'

    if (contract.constructorArgs.length) {
      args = (await $`cast abi-encode constructor(address) ${contract.constructorArgs.join(' ')}`).stdout
    }

    console.log(`Verifying ${name} at ${contract.address} with args ${args}`)

    await $`forge verify-contract ${contract.address} ${name} --constructor-args ${args} --chain-id ${chainId} --verifier etherscan --etherscan-api-key ${process.env.ETHERSCAN_API_KEY} --watch`

    console.log(`Verified!`)
  }
})()
```

Inside the Gemforge configuration we then set this script as a post-deploy hook:

```js
// file: gemforge.config.js
module.exports = {
  // ...
  hooks: {
    // ...
    postDeploy: 'scripts/verify.js',
  }, 
  // ... 
}
```

And that's it! Every time we deploy, this script will be called.

!!!
The `GEMFORGE_DEPLOY_CHAIN_ID` environment variable relied upon by the verification script is set automatically by Gemforge when calling deployment hooks. This allows the script to know which network is being deployed to.
!!!