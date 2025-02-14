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
    // Names of core facet contracts - these will not be modified/removed once deployed.
    // This default list is based on the diamond-2-hardhat library.
    // NOTE: WE RECOMMEND NOT CHANGING ANY OF THESE EXISTING NAMES UNLESS YOU KNOW WHAT YOU ARE DOING.    
    coreFacets: [
      'OwnershipFacet',
      'DiamondCutFacet',
      'DiamondLoupeFacet',
    ],
    // Function selectors that should NEVER be removed from the diamond.
    // The default list is all the external methods of the default list of core facets defined above.
    // NOTE: This is an array of function selectors, not method names.
    protectedMethods: [
      '0x8da5cb5b', // OwnershipFacet.owner()
      '0xf2fde38b', // OwnershipFacet.transferOwnership()
      '0x1f931c1c', // DiamondCutFacet.diamondCut()
      '0x7a0ed627', // DiamondLoupeFacet.facets()
      '0xcdffacc6', // DiamondLoupeFacet.facetAddress()
      '0x52ef6b2c', // DiamondLoupeFacet.facetAddresses()
      '0xadfca15e', // DiamondLoupeFacet.facetFunctionSelectors()
      '0x01ffc9a7', // DiamondLoupeFacet.supportsInterface()
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

## Protected methods

Protected methods are similar to core facets in that they will NEVER be removed from the Diamond during an upgrade. This allows you to protect specific methods in your own facets. The default list contains the external methods in the core facets as additional protection.

Note, however, that protected methods can still be overridden and replaced.

!!!
**Unless you really know what you're doing, we recommend leaving `coreFacets` and `protectedMethods` unchanged.**
!!!

