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
    // Base Sepolia test network
    base_sepolia: {
      // RPC endpoint URL
      rpcUrl: () => process.env.BASE_SEPOLIA_RPC_URL,
      // OPTIONAL: Contract source code verification - takes place as soon as any contract is deployed by Gemforge
      contractVerification: {
        // if using Foundry
        foundry: {
          // URL to block explorer contract source submission API
          apiUrl: 'https://api-sepolia.basescan.org/api',
          // secret API key to use when submitting
          apiKey: () => process.env.BASESCAN_API_KEY,
        },
        // if using Hardhat
        hardhat: {
          networkId: 'baseSepolia' // name of network as defined in hardhat config file
        },
      },
    },
  },
  ...
}
```

The `rpcUrl` value can either be a URL string or a method which returns the same.