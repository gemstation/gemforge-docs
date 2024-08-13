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
    // OPTIONAL - proxy contract (DiamondProxy.sol) options
    proxy: {
      // custom template to use instead of the Gemforge default one
      template: '/path/to/custom-DiamondProxy-template.sol'
    },
    // proxy interface options
    proxyInterface: {
      // imports to include in the generated IDiamondProxy interface
      imports: [],
    },
  },
  ...
}
```

## Proxy template

By default Gemforge uses its own [DiamondProxy.sol template](https://github.com/gemstation/gemforge/blob/master/templates/DiamondProxy.sol) to generate the final proxy contract code.

However, advanced users may wish to have more control, including using custom upgrade facets. In these cases, set the `generator.proxy.template` key to point to a custom template file to use instead of the default. 

We recommend basing custom proxy templates on the Gemforge default one and making only the necessary modifications. Examples can be found in the [Gemforge test codebase](https://github.com/gemstation/gemforge/tree/master/test/data/test-templates).

!!!
Please be careful when using a custom proxy template. Gemforge expects various default facets and methods to be present on a deployed Diamond in order to be able to query it and perform upgrades successfully.
!!!


## Proxy interface imports

The `generator.proxyInterface.imports` array lists files which should be imported within the [generated diamond proxy interface](../commands/build.md) code.

For example, you need to use this when dealing with [custom structs](../development/custom-structs.md).


