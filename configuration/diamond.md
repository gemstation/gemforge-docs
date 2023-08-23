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
  },
  ...
}
```

Note that the initialization contract - `diamond.init` config - must be a uniquely named contract so that Gemforge can easily find it in the artifacts folder.
