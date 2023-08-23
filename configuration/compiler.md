---
order: -1
---

# Compiler

All generated Solidity code will have the following two lines at the top:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >=0.8.21;
```

Both the SPDX license id and Solidity version to use must be set in the config file:

```js
// gemforge.config.cjs
module.exports = {
  ...
  solc: {
    // SPDX License - to be inserted in all generated .sol files
    license: 'MIT',
    // Solidity compiler version - to be inserted in all generated .sol files
    version: '0.8.21',
  },
  ...
}
```

The SPDX license id must be valid, and will be checked against a known list.
