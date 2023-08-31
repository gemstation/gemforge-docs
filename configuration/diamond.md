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
    // The diamond initialization contract - to be called when first deploying the diamond.
    init: 'InitDiamond',
    // Names of core facet contracts - these will not be modified/removed once deployed and are also reserved names.
    // This default list is taken from the diamond-2-hardhat library.
    // NOTE: we recommend not removing any of these existing names unless you know what you are doing.
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

The initialization contract - `diamond.init` config - must be a uniquely named contract so that Gemforge can easily find it in the artifacts folder. 

If this value is empty then an initialization call will not be made.

## Core facets

Core facets are contracts whose selectors are considered sacrosanct such that they will never be removed from the diamond in upgrade by Gemforge. Gemforge will also complain if you create a contract that has one of these names.

The default core facets in the config file are the ones provided by the [standard diamond library](https://github.com/mudgen/diamond-2-hardhat) and are essential to the correct working of the deployment and upgrade mechanisms:

* `DiamondLoupe` - An interface for querying the diamond. Essential to making upgrades work.
* `DiamondCut` - The upgrade mechanism for the diamond.
* `Ownership` - Verification and transfer of diamond ownership.

**Unless you really know what you're doing we recommend leaving this array unchanged.**