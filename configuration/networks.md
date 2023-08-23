---
order: -9
---

# Networks

The available deployment networks must be explicitly specified in the configuration. Gemforge does not rely on any networks you've configured elsewhere in your development environment.

```js
// gemforge.config.cjs
module.exports = {
  ...
  networks: {
    // Local network
    local: {
      // RPC endpoint URL
      rpcUrl: 'http://localhost:8545',
      // Wallet to use for deployment
      wallet: 'wallet1',
    },
    // Sepolia test network
    sepolia: {
      // RPC endpoint URL
      rpcUrl: () => process.env.SEPOLIA_RPC_URL,
      // Wallet to use for deployment
      wallet: 'wallet2',
    }
  }
  ...
}
```

The `wallet` value must refer to a valid, defined [wallet](./wallets.md). If not then Gemforge will complain.

To deploy contracts to one of the specified networks simply use it's name with the [deploy](../commands/deploy.md) command, e.g to deploy to the `sepolia` network defined in the configuration file:

```shell
gemforge deploy sepolia
```