---
order: -2
---

# Initialization

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

Sometimes you may wish to run some initialization code during an upgrade. To do this you can use the [`--upgrade-init-*` parameters](../commands/deploy.md) as follows:

```shell
gemforge deploy <target>
  --upgrade-init-contract <contract name> 
  --upgrade-init-method <contract method name>
```

Gemforge will automatically check the passed-in details, deploy the named contract, and then execute the named method within the context of the Diamond. **Note: It is expected that the contract method takes no arguments.**

We recommend placing such upgrade initialization code within the `init` subfolder, alongside your initialization code for fresh deployments:

```
/init    <--- here
  init/InitDiamond    <-- to run on fresh deployments
  init/UpgradeInit1   <-- the first upgrade initialization
  init/UpgradeInit2   <-- the second upgrade initialization
  init/...
/facets
/shared
/generated 
...
```

We recommend that you use [storage variables](./storage.md) to keep track of whether a given upgrade initialization code has previously been executed. This will ensure you can't run the same code twice, thereby preventing any mistakes. For example:

```solidity
// file: src/init/UpgradeInit1.sol

// SPDX-License-Identifier: MIT
pragma solidity >=0.8.21;

import { AppStorage, LibAppStorage } from "../libs/LibAppStorage.sol";

contract UpgradeInit1 {
  function init() external {
    AppStorage storage s = LibAppStorage.diamondStorage();
    if (s.upgradeInit1Executed) {
      revert("UpgradeInit1 already executed");
    }
    s.upgradeInit1Executed = true;

    // do more stuff here...
  }
}
```
