---
order: -9
---

# Networks

Networks (i.e. chains) are required for deploying to a [target](./targets.md).

They are configured as follows:

```js
// gemforge.config.cjs
module.exports = {
  ...
  networks: {
    // Local network
    local: {
      // RPC endpoint URL
      rpcUrl: 'http://localhost:8545',
    },
    // Sepolia test network
    sepolia: {
      // RPC endpoint URL
      rpcUrl: () => process.env.SEPOLIA_RPC_URL,
    }
  }
  ...
}
```

The `rpcUrl` value can either be a URL string or a method which returns the same.