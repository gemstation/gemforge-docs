---
order: -2
---

# build

The `build` command does all of the following:

* Auto-generates Diamond proxy code and Dapp interface.
* Auto-generates Diamond deployment code for testing in Foundry.
* Builds/compiles all your code.

To run it:

```shell
gemforge build
```

The following files will be generated:

* `DiamondProxy.sol` - the core diamond proxy contract which delegates to all of your facets. The constructor sets up the [core library facets](https://github.com/mudgen/diamond-2-hardhat/tree/main/contracts/facets), as well as a basic [ERC165](https://eips.ethereum.org/EIPS/eip-165) implementation.
* `IDiamondProxy.sol` - the diamond proxy interface using which Dapps can execute facet methods on the on-chain contract.
* `LibDiamondHelper.sol` - a library for use within Foundry tests for deploying the Diamond and its facets.
* `facets.json` - a list of all facets and their exported methods - used by the [deploy](deploy.md) command.

!!!
We recommend adding all of the above files to your `.gitignore` since they are not meant to be committed to version control.
!!!

To see an example of the above generated files in action check out the [sample project](scaffold.md) command.

## Custom scripts

If you wish to run custom scripts during the build process, then this can be accomplished using [hooks](../configuration/hooks.md). Hooks are custom scripts written in a language of your choice which execute pre- and/or post-build.

See the section on [verifying your contracts on Etherscan](../development/etherscan.md) for an example of hooks in action.
