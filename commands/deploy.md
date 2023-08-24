---
order: -3
---

# deploy

The `deploy` command both deploys **and** upgrades Diamond contracts on a network.

If no previous deployment is detected (see below) it will do a new one, otherwise it will automatically work out which facets and methods have changed (by comparing to the on-chain bytecode) and upgrade the on-chain proxy accordingly.

To run it:

```shell
gemforge deploy
```

This will deploy contracts to the `local` network, which is defined in the default configuration file. 

To deploy to another network, first ensure that [network is defined](../configuration/networks.md) in the configuration. Then run:

```
gemforge deploy <network name>
```

## Deployment records

Once deployment has succeeded a `gemforge.deployments.json` file will be created. This contains the addresses of all the deployed contracts along with their constructor arguments and deployment transaction hashes, for each network that has been deployed to. 

An example `gemforge.deployments.json`:

```json
{
  // local
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
    // ...
  ],
  // mainnet
  "1": [
    // ...
  ]
}
```

Subsequent calls to the `deploy` command will result in this file being checked to see if a fresh deployment is needed, or if an upgrade should be performed on the given network.

Gemforge performs a thorough check, i.e. it actually attempts to query the proxy contract on the network to see if it is indeed a Diamond deployment.

If instead, you wish to force a fresh deployment, then simply append the `-n` or `--new` option to the command:

```shell
gemforge deploy -n
```

## Etherscan verification

To verify your contracts on Etherscan you can use [post-deploy hooks](../configuration/hooks.md). Examples of this can be found in the 
example project repos:

* Foundry: https://github.com/gemstation/contracts-foundry
* Hardhat: https://github.com/gemstation/contracts-hardhat
