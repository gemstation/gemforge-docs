---
order: -6
---

# Diamond

The diamond structure itself can be configured:

```js
// gemforge.config.cjs
module.exports = {
  ...
  diamond: {
    // Whether to include public methods when generating the IDiamondProxy interface. Default is to only include external methods.
    publicMethods: false,
    // OPTIONAL - The diamond initialization contract - to be called when first deploying the diamond.
    init: {
      // The diamond initialization contract name
      contract: 'InitDiamond',
      // The diamond initialization function name
      function: 'init',
    },  
    // Names of core facet contracts - these will not be modified/removed once deployed and are also reserved names.
    // This default list is taken from the diamond-2-hardhat library.
    // NOTE: WE RECOMMEND NOT CHANGING ANY OF THESE EXISTING NAMES UNLESS YOU KNOW WHAT YOU ARE DOING.
    coreFacets: [
      'OwnershipFacet',
      'DiamondCutFacet',
      'DiamondLoupeFacet',
    ],
  },
  ...
}
```

## Initialization contract

The initialization contract - `diamond.init` config - is optional, and is only needed if you wish to execute [custom initialization code](../development/initialization.md) on-chain when doing a new deployment of your Diamond.

## Core facets

Core facets are contracts whose selectors are considered sacrosanct such that they will never be removed from the diamond in upgrade by Gemforge. Gemforge will also complain if you create a contract that has one of these names.

The default core facets in the config file are the ones provided by the [standard diamond library](https://github.com/mudgen/diamond-2-hardhat) and are essential to the correct working of the deployment and upgrade mechanisms:

* `DiamondLoupe` - An interface for querying the diamond. Essential to making upgrades work.
* `DiamondCut` - The upgrade mechanism for the diamond.
* `Ownership` - Verification and transfer of diamond ownership.

!!!
**Unless you really know what you're doing, we recommend leaving this array unchanged.**
!!!