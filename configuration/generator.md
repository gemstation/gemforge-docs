---
order: -4
---

# Generator

Generator options modify the behaviour of the code generator. 

```js
// gemforge.config.cjs
module.exports = {
  ...
  generator: {
    // proxy interface options
    proxyInterface: {
      // imports to include in the generated IDiamondProxy interface
      imports: [],
    },
  },
  ...
}
```

## Proxy interface imports

The `generator.proxyInterface.imports` array lists files which should be imported within the [generated diamond proxy interface](../commands/build.md) code.

For example, you need to use this when dealing with [custom structs](../advanced/custom-structs.md).

