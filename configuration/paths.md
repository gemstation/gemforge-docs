---
order: -3
---

# Paths

Gemforge needs to know where to find your contract source code, library imports, build artifacts, and where to place all generated output.

The configuration options:

```js
// gemforge.config.cjs
module.exports = {
  ...
  paths: {
    // contract built artifacts folder
    artifacts: 'out',
    // source files
    src: {
      // file patterns to include in facet parsing
      facets: [
        // include all .sol files in the facets directory ending "Facet"
        'src/facets/*Facet.sol'
      ],
    },
    // folders for gemforge-generated files
    generated: {
      // output folder for generated .sol files
      solidity: 'src/generated', 
      // output folder for support scripts and files
      support: '.gemforge',
      // deployments JSON file
      deployments: 'gemforge.deployments.json',
    },
    // library source code
    lib: {
      // diamond library
      diamond: 'lib/diamond-2-hardhat',
    }
  },
  ...
}
```

