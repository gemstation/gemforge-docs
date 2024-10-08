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
      // OPTIONAL - CREATE3 salt. If empty or ommitted a random SALT is used.
      create3Salt: '0x...'
    },
    // Target named "testnet"
    testnet: {
      // Network to deploy to
      network: 'sepolia',
      // Wallet to use for deployment
      wallet: 'wallet2',
      // Initialization method arguments
      initArgs: [],
      // OPTIONAL - CREATE3 salt. If empty or ommitted a random SALT is used.
      create3Salt: '0x...'
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

## CREATE3 salt

The `create3Salt` value is a `"0x"` prefixed hex string of length `66` that is to be used for [keyless CREATE3 deployments](https://ethereum-magicians.org/t/keyless-contract-deployment-with-create3/16025) of the Diamond proxy contract.

By setting this value you can ensure that your Diamond proxy contract is always deployed to the same address as long as the deployment wallet and salt value remain unchanged. And by using the same wallet and salt for every deployment target you can ensure that all deployments to all targets (and thus chains) are at the same address, making managing [omni-chain deployments](../development/omni-chain-addresses.md) easy.

If the salt value is empty or ommitted then Gemforge will create a random salt to use, meaning that fresh deployments of your Diamond proxy will always be at different addresses.

_Note: Only the Diamond proxy contract is deployed using `CREATE3`. Facet contracts are deployed normally._


