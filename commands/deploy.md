---
order: -3
---

# deploy

The `deploy` command both deploys **and** upgrades Diamond contracts on a network.

If no previous deployment is detected (see below) it will do a new one, otherwise it will automatically work out which facets and methods have changed (by comparing to the on-chain bytecode) and upgrade the on-chain proxy accordingly.

To run it:

```shell
gemforge deploy <target>
```

This will deploy contracts for the specified `<target>`, which must be defined in the [targets configuration](../configuration//targets.md). 

## Deployment records

Once deployment has succeeded a `gemforge.deployments.json` file will be created. This contains the addresses of all the deployed contracts along with their constructor arguments and deployment transaction hashes, for each target that has been deployed to. 

An example `gemforge.deployments.json`:

```json
{
  "local": {
    "chainId": 31337,
    "contracts": [
      {
        "name": "DiamondProxy",
        "fullyQualifiedName": "contracts/generated/DiamondProxy.sol:DiamondProxy",
        "sender": "0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266",
        "txHash": "0x4a304d29c18e4e3d9e8194f63d1d4e484304b9079f0d390ac3c06a8790c1494b",
        "onChain": {
          "address": "0x5FbDB2315678afecb367f032d93F642f64180aa3",
          "constructorArgs": [
            "0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266"
          ]
        }
      },
      // ...
    ]
  },
  "testnet": {
    "chainId": 11155111,
    "contracts": [
      {
        "name": "DiamondProxy",
        "fullyQualifiedName": "contracts/generated/DiamondProxy.sol:DiamondProxy",
        "sender": "0xb1B6e377aA6ec6928A1D499AE58483B2B99658Ec",
        "txHash": "0xf2e22d2e01c637cfb0f8996a762574ed15046d5ad745f826b547ee74e1e67f4b",
        "onChain": {
          "address": "0xA91C335D79b8401E1df5FA9D5C1D441f963F8eB0",
          "constructorArgs": [
            "0xb1B6e377aA6ec6928A1D499AE58483B2B99658Ec"
          ]
        }
      },
      // ...
    ]
  }
}
```

Subsequent calls to the `deploy` command with the same [target](../configuration/targets.md) will result in this file being checked to see if a deployment already exists for the given target and can thus be upgraded, or if a new deployment is needed.

When checking for an existing deployment, Gemforge performs a thorough check - i.e. it actually attempts to query the proxy contract on the target's network and to see if it is indeed a Diamond deployment.

!!!
In order for upgrades to work, Gemforge assumes that the core facets - [DiamondLoupe](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/DiamondLoupeFacet.sol), [DiamondCut](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/DiamondCutFacet.sol), [Ownership](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/OwnershipFacet.sol) - are part of your diamond. If you used Gemforge to deploy the initial diamond then this will already be taken care of for you.
!!!

## Fresh deployments

You can bypass Gemforge's default behaviour and force a fresh deployment of the Diamond using the `--new` CLI argument:

```shell
gemforge deploy <target> --new
```

This will force a new deployment of the Diamond and associated facets, disregarding any existing on-chain Diamond. Existing deployment records for the Diamond contract as well as facets will be replaced with new ones.

Sometimes you may simply wish to reset an existing Diamond to a fresh state, i.e. replace all of its selectors with the current facet build output. To do this you can use the `--reset` argument:

```shell
gemforge deploy <target> --reset
```

This will remove all non-core facet selectors from the existing on-chain Diamond as a first step, thus causing the current facet contracts to be deployed to the diamond afresh. The existing deployment record for the Diamond contract will remain unchanged, but facet deployment records will be replaced.

## Custom upgrade initializations

Sometimes it's useful to be able to run "initialization" code during an upgrade of your existing Diamond, similar to the [code that gets run during a fresh deployment](../development/initialization.md).

To accomplish this with `--upgrade-init-*` parameters can be used:

```shell
gemforge deploy <target> 
  --upgrade-init-contract <contract name> 
  --upgrade-init-method <contract method name>
```

Gemforge will automatically check the passed-in details, deploy the named contract, and then execute the named method within the context of the Diamond. Note that the contract method must not take any arguments. 

!!!
You can "upgrade" your Diamond and run upgrade initialization code even if you're not making any changes to the facets!
!!!

## Dry runs

Sometimes you may want to see what a deployment would do without actually going through with it. This is known as a _dry_ deployment and can be actioned using the `--dry` option. For example:

```shell
gemforge deploy <target> --dry
```

No actual deployments will take place but Gemforge will still output useful logging to tell you what it would do.

The dry deployment option also works for when doing upgrades, resetting a diamond or doing a fresh deployment to the target.

## Pause and resume

When doing deployments it's possible to pause the flow and save the state to file, to be resumed later. This is useful if e.g you have a custom authentication flow which requires you to approve upgrades.

To pause a deployment:

```shell
gemforge deploy <target> --pause-cut-to-file cut.json
```

Running this command will deploy all the proxy and facet contracts, but the [diamondCut()](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/DiamondCutFacet.sol#L22) method will not be called. The `cut.json` file will contain the array of facet cuts that would be passed to the proxy to complete the deployment/upgrade, e.g:


```js
{
  "cuts": [
    {
      "facetAddress": "0x0000000000000000000000000000000000000000",
      "action": 2,
      "functionSelectors": [
        "0xbf9f7311"
      ]
    },
    // ...
 ],
  "initContractAddress": "0x0000000000000000000000000000000000000000",
  "initData": "0x"
}
```

To resume the deployment at any point, run:

```shell
gemforge deploy <target> --resume-cut-from-file cut.json
```

_Note: Post-build [hooks](../configuration/hooks.md) will only be called if the resume step succeeds. These hooks will not be called for the pause step_.

## Custom scripts

If you wish to run custom scripts during the deployment process, then this can be accomplished using [hooks](../configuration/hooks.md). Hooks are custom scripts written in a language of your choice which execute pre- and/or post-deployment.

See the section on [verifying your contracts on Etherscan](../development/etherscan.md) for an example of hooks in action.