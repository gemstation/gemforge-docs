---
order: -10
---

# Targets

Targets are how Gemforge [deploys](../commands/deploy.md) knows how to deploy your contracts.

Each target must specify a [network](./networks.md) and [wallet](./wallets.md) to use for deployment. 

This means you can deploy multiple instances of your Diamond to the same network, where each instance is represented by a different target.

```js
// gemforge.config.cjs
module.exports = {
  ...
  targets: {
    // Target named "local"
    local: {
      // Network to deploy to
      network: 'local',
      // Wallet to use for deployment
      wallet: 'wallet1',
      // Initialization method arguments
      initArgs: []
    },
    // Target named "testnet"
    testnet: {
      // Network to deploy to
      network: 'sepolia',
      // Wallet to use for deployment
      wallet: 'wallet2',
      // Initialization method arguments
      initArgs: []
    }
  }
  ...
}
```

The `wallet` value must refer to a valid, defined [wallet](./wallets.md). The `initArgs` value is only valid if you are [customizing the initialization](../development/initialization.md) of your diamond.

To deploy contracts to one of the specified targets simply use its name with the [deploy](../commands/deploy.md) command. For example, to deploy to the `testnet` target:

```shell
gemforge deploy testnet
```