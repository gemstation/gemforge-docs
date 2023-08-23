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
    // library files
    lib: {
      // diamond library
      diamond: 'lib/diamond-2-hardhat',
    }
  },
  ...
}
```

## Generated files

The `generated.solidity` folder path should be inside the same folder containing your project's smart contracts so that everything gets compiled together.

The `generated.solidity` folder path is for holding files which are used by Gemforge internally.

The `generated.deployments` file will contain the details of deployed Diamond contracts, indexed by network chain id. You may wish to check this file into version control in order to store these details for consumption elsewhere.

## Library files

The `lib.diamond` folder path should point to where the contents of the [diamond-2-hardhat](https://github.com/mudgen/diamond-2-hardhat) repository can be found. This is required for the code which Gemforge generates.

