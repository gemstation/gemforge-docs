---
order: -3
---

# deploy

The `deploy` command deploys the Diamond contracts to the selected network. 

If no previous deployent can be detected it will do a new one, otherwise it will automatically work out which facets and methods have changed (by comparing to the on-chain bytecode) and upgrade the on-chain Diamond proxy accordingly.

To run it:

```shell
gemforge deploy
```

This will deploy contracts to the `local` network, which is defined in the default configuration file. 

To deploy to another network, first ensure that [network is defined](../configuration/networks.md) in the configuration. Then run:

```
gemforge deploy <network name>
```

Once deployment has succeeded a `gemforge.deployments.json` file will be created. This will contain the addresses of all the deployed contracts (proxy, facets, initialization contract) along with their constructor arguments and deployment transaction hashes. 

This file can then be committed to version control and/or used in scripts or NPM package exports, making it easy for clients of your contracts to know where to find your contracts for a given chain.

An example `gemforge.deployments.json`:

```json
{
  "31337": [
    {
      "name": "DiamondProxy",
      "fullyQualifiedName": "DiamondProxy.sol:DiamondProxy",
      "sender": "0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266",
      "txHash": "0x106fc779d5317edc3947fff0e1ee659e578ea5d59d2910059e7f14b658c4c460",
      "contract": {
        "address": "0x5FbDB2315678afecb367f032d93F642f64180aa3",
        "constructorArgs": [
          "0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266"
        ]
      }
    },
    {
      "name": "ERC20Facet",
      "fullyQualifiedName": "ERC20Facet.sol:ERC20Facet",
      "sender": "0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266",
      "txHash": "0x6caf1baeee6815a547e611404eea1c303f9142e4a5a08a9b8e97ab5fbdcc903a",
      "contract": {
        "address": "0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512",
        "constructorArgs": []
      }
    },
    {
      "name": "InitDiamond",
      "fullyQualifiedName": "InitDiamond.sol:InitDiamond",
      "sender": "0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266",
      "txHash": "0x89eb25a81f46ac3be515e5aaaba385a6eaeb1c30a26d7cfff72aad090c99623a",
      "contract": {
        "address": "0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0",
        "constructorArgs": []
      }
    }
  ]
}
```