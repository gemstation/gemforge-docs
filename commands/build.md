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

* `DiamondProxy.sol` - the core Solidity contract which delegates to all of your facets.
* `IDiamondProxy.sol` - the Solidity interface using which Dapps can execute facet methods on the on-chain contract.
* `LibDiamondHelper.sol` - a Solidity library for use within Foundry tests for deploying the Diamond and its facets.
* `facets.json` - a list of all facets and their exported methods - used by the [deploy](deploy.md) command.

!!!
We recommend adding all of the above files to your `.gitignore` since they are not meant to be committed to version control.
!!!

To see an example of the above generated files in action check out the [sample project](scaffold.md) command.
