---
order: -2
---

# Custom initialization

When deploying a Diamond for the first time, it's possible to run additional initialization code at the end of [the cut call](https://github.com/mudgen/diamond-2/blob/master/contracts/facets/DiamondCutFacet.sol#L20), in order to setup your Diamond's internal state.

This is specified as a specific contract and method to call within the [configuration file](../configuration/diamond.md). For example:

```js
// file: gemforge.config.cjs
module.exports = {
  // ...
  diamond: {
    // ...
    // custom initialization call
    init: {
      // The diamond initialization contract name
      contract: 'InitDiamond',
      // The diamond initialization function name
      function: 'init',
    },  
    // ...
  },
  // ...
}
```

We would then create an `InitDiamond.sol` file somewhere within our source tree, and it must contain the `init()` method which may or may not take arguments. For this example, let's have a single `uint` argument:

```solidity
// file: src/init/InitDiamond.sol

// SPDX-License-Identifier: MIT
pragma solidity >=0.8.21;

import { AppStorage, LibAppStorage } from "../libs/LibAppStorage.sol";

contract InitDiamond {
  function init(uint initialValue) external {
    AppStorage storage s = LibAppStorage.diamondStorage();

    // update storage here
    s.var1 = initialValue
    // s.var2 = ...
  }
}
```

When deploying to any [target](../configuration/targets.md) we will need to specify the values for the `init()` method arguments. This allows to specify different initialization parameters (and thus initialize our Diamond different) for different target.

For example, for the above `init()` method we need to specify one argument:

```js
// file: gemforge.config.cjs
module.exports = {
  ...
  targets: {
    local: {
      network: 'local',
      wallet: 'wallet1',
      initArgs: [1] // initialValue = 1
    },
    testnet: {
      network: 'sepolia',
      wallet: 'wallet2',
      initArgs: [2] // initialValue = 2
    }
  }
  ...
}
```

## Initialization during an upgrade

Sometimes you may wish to run some initialization code during an upgrade. To do this will require using the ["pause and resume"](../commands/deploy.md) deployment flow. 

When pausing a deployment a JSON file is created that contains the data for the `diamondCut()` call, e.g:

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

The `initContractAddress` and `initData` values get passed to the `diamondCut()` call. By modifying them you can run custom initialization code when the deployment is resumed. 

_Note: The `initData` value must the ABI-encoded function selector and arguments to be passed to it._