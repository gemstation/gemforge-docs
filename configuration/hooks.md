---
order: -7
---

# Hooks

Gemforge makes it easy to run pre- and post- hooks for the [build](../commands/build.md) and [deploy](../commands/deploy.md) commands. These can be any command-line call:

```js
// gemforge.config.cjs
module.exports = {
  ...
  hooks: {
    // shell command to execute before build
    preBuild: 'echo "pre-build"',
    // shell command to execute after build
    postBuild: 'echo "post-build"',
    // shell command to execute before deploy
    preDeploy: 'echo "pre-deploy"',
    // shell command to execute after deploy
    postDeploy: 'echo "post-deploy"',
  },
  ...
}
```

## Environment variables

When Gemforge calls the `preDeploy` and `postDeploy` hooks it will pass through the following environment variables:

* `GEMFORGE_DEPLOY_CHAIN_ID` - chain id of network being deployed to

This can then be used in your hooks, e.g we may define a post-deployment script as follows:


```js
// gemforge.config.cjs
module.exports = {
  ...
  hooks: {
    postDeploy: 'node ./postDeployScript.js',
  }
  ...
}

// postDeployScript.js
console.log(`Deployed to chain: ${process.env.GEMFORGE_DEPLOY_CHAIN_ID}`);
```

See the section on [verifying your contracts on Etherscan](../advanced/etherscan.md) for a more detailed example of hooks.

!!!
If any of the hook scripts fail to execute properly or throw an error then Gemforge will also throw an error.
!!!
