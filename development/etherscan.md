---
order: -4
---

# Etherscan verification

To verify your contracts on Etherscan you can use [post-deploy hooks](../configuration/hooks.md).

We first need a script to perform the verification. Our requirements:

* Load deployment information from the [deployment records](../commands/deploy.md).
* Load deployment network information from the [hook environment](../configuration/hooks.md).
* Only verify for public chains, and not local test nodes.

Here is the [Foundry example project script](https://github.com/gemstation/contracts-foundry/blob/master/scripts/verify.js):

```js
#!/usr/bin/env node
(async () => {
  require('dotenv').config()
  const { $ } = (await import('execa'))

  const deploymentInfo = require('../gemforge.deployments.json')

  const target = process.env.GEMFORGE_DEPLOY_TARGET
  if (!target) {
    throw new Error('GEMFORGE_DEPLOY_TARGET env var not set')
  }

  // skip localhost
  if (target === 'local') {
    console.log('Skipping verification on local')
    return
  }

  console.log(`Verifying for target ${target} ...`)

  const contracts = (deploymentInfo[target] || {}).contracts || []

  for (let { name, onChain } of contracts) {
    let args = '0x'

    if (onChain.constructorArgs.length) {
      args = (await $`cast abi-encode constructor(address) ${onChain.constructorArgs.join(' ')}`).stdout
    }

    console.log(`Verifying ${name} at ${onChain.address} with args ${args}`)

    await $`forge verify-contract ${onChain.address} ${name} --constructor-args ${args} --chain-id ${deploymentInfo[target].chainId} --verifier etherscan --etherscan-api-key ${process.env.ETHERSCAN_API_KEY} --watch`

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
The `GEMFORGE_DEPLOY_TARGET` environment variable relied upon by the verification script is set automatically by Gemforge when calling [deployment hooks](../configuration/hooks.md).
!!!