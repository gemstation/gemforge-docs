---
order: -4
---

# Source-code verification

Gemforge has **built-in support** for getting your deployed contracts verified with source-code on Etherscan and similar block explorers.

This will give people confidence in knowing what your contracts actually do and also allow for direct interaction via a block explorer.

When defining a [network](../configuration/networks.md) in the Gemforge configuration the presence of a `contractVerification` key instructs Gemforge to verify contract source code when deploying contracts to that network: 

```js
// File: gemforge.config.cjs
module.exports = {
  ...
  networks: {
    ...
    // Base Sepolia test network
    base_sepolia: {
      // RPC endpoint URL
      rpcUrl: 'https://sepolia.base.org',
      // Contract source code verification will take place for targets using this network if this key is present!
      contractVerification: {
        // ... configuration here!
      },
    },
  },
  ...
}
```

Gemforge will only perform contract source verification if this key is present.

!!!
If a failure occurs during contract source verification then Gemforge will not proceed any further with the deployment and will instead throw an error.
!!!

## Foundry

For Foundry the configuration must look like this:

```js
// File: gemforge.config.cjs
module.exports = {
  ...
  networks: {
    ...
    // Base Sepolia test network
    base_sepolia: {
      // RPC endpoint URL
      rpcUrl: 'https://sepolia.base.org',
      // Contract source code verification will take place for targets using this network
      contractVerification: {
        // if using Foundry
        foundry: {
          // URL to block explorer contract source submission API
          apiUrl: 'https://api-sepolia.basescan.org/api',
          // secret API key to use when submitting (get this by signing up on Basescan.org)
          apiKey: () => process.env.BASESCAN_API_KEY,
        },
      },
    },
  },
  ...
}
```

## Hardhat

For Hardhat the configuration must look like this:

```js
// File: gemforge.config.cjs
module.exports = {
  ...
  networks: {
    ...
    // Base Sepolia test network
    base_sepolia: {
      // RPC endpoint URL
      rpcUrl: 'https://sepolia.base.org',
      // Contract source code verification will take place for targets using this network
      contractVerification: {
        // if using hardhat
        hardhat: {
          // name of network as defined in the hardhat configuration file
          networkId: 'baseSepolia'
        },
      },
    },
  },
  ...
}
```

The `networkId` value refers to the name of the network as defined in the Hardhat configuration file. For the above example it would look like this:

```js
// File: hardhat.config.ts
require("dotenv").config();
import type { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";

const config: HardhatUserConfig = {
  solidity: "0.8.21",
  networks: {
    baseSepolia: {
      url: 'https://sepolia.base.org',
    }
  },
  etherscan: {
    apiKey: {
      baseSepolia: process.env.BASESCAN_API_KEY
    },
    customChains: [
      {
        network: "baseSepolia",
        chainId: 84532,
        urls: {
          apiURL: "https://api-sepolia.basescan.org/api",
          browserURL: "https://sepolia.basescan.org"
        }
      }
    ]    
  },
};

export default config;
```

## Example projects

Both the Foundry and Hardhat sample projects contain working examples of contract source verification:

* Foundry: https://github.com/gemstation/contracts-foundry
* Hardhat: https://github.com/gemstation/contracts-hardhat
