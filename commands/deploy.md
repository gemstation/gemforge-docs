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

Subsequent calls to the `deploy` command will result in this file being checked to see if a deployment already exists on the given network and can thus be upgraded, or if a new deployment is needed.

When checking for an existing deployment, Gemforge performs a thorough check - i.e. it actually attempts to query the proxy contract on the network and to see if it is indeed a Diamond deployment.

!!!
In order for upgrades to work, Gemforge assumes that the core facets - [DiamondLoupe](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/DiamondLoupeFacet.sol), [DiamondCut](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/DiamondCutFacet.sol), [Ownership](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/OwnershipFacet.sol) - are part of your diamond. If you used Gemforge to deploy the initial diamond then this will already be taken care of for you.
!!!

## Fresh deployments

You can bypass Gemforge's default behaviour and force a fresh deployment of the Diamond using the `--new` CLI argument:

```shell
gemforge deploy --new
```

This will force a new deployment of the Diamond and associated facets, disregarding any existing on-chain Diamond. Existing deployment srecord for the Diamond contract as well as facets will be replaced with new ones.

Sometimes you may simply wish to reset an existing Diamond to a fresh state, i.e. replace all of its selectors with the current facet build output. To do this you can use the `--clean` argument:

```shell
gemforge deploy --clean
```

This will remove all non-core facet selectors from the existing on-chain Diamond as a first step, thus causing the current facet contracts to be deployed to the diamond afresh. The existing deployment record for the Diamond contract will remain unchanged, but facet deployment records will be replaced.

## Custom scripts

If you wish to run custom scripts during the deployment process, then this can be accomplished using [hooks](../configuration/hooks.md). Hooks are custom scripts written in a language of your choice which execute pre- and/or post-deployment.

See the section on [verifying your contracts on Etherscan](../advanced/etherscan.md) for an example of hooks in action.