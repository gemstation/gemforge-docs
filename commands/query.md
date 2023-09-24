---
order: -4
---

# query

The `query` command outputs the facet configuration of an existing Diamond contract on a network.

To run it:

```shell
gemforge query <target>
```

This will query the deployment records to find the currently deployed contracts for the specified `<target>` which must be defined in the [targets configuration](../configuration//targets.md). 

It then matches the on-chain facet and selector information to the current [built contract artifacts](./build.md). For any facets and functions which are not recognized it will note this in the final output.

The output will look something like:

```shell
Diamond: 0x...

Unrecognized facets: 0
Unrecognized functions: 0

DiamondCutFacet (0x...)
  fn: diamondCut (0x1f931c1c)
DiamondLoupeFacet (0x...)
  fn: facets (0x7a0ed627)
  fn: facetFunctionSelectors (0xadfca15e)
  fn: facetAddresses (0x52ef6b2c)
  fn: facetAddress (0xcdffacc6)
  fn: supportsInterface (0x01ffc9a7)
OwnershipFacet (0x...)
  fn: owner (0x8da5cb5b)
  fn: transferOwnership (0xf2fde38b)
ExampleFacet (0x...)
  fn: getInt1 (0xe1bb9b63)
  fn: setInt1 (0x4d2c097d)
```

For any unrecognized facets and/or functions the label `<unknown>` will be used in the output, e.g:

```shell
<unknown> (0x...)
  fn: <unknown> (0xe1bb9b63)
  fn: <unknown> (0x4d2c097d)
```

## JSON output

A JSON version of the result can be obtained using the `--json` CLI option. This will produce output similar to:

```js
{
  "proxyAddress": "0x...",
  "unrecognizedFacets": 0,
  "unrecognizedFunctions": 0,
  "facets": {
    "DiamondCutFacet": {
      "address": "0x...",
      "functions": [
        {
          "name": "diamondCut",
          "selector": "0x1f931c1c"
        }
      ]
    },
    // ...
  }
}
```

## Output to file

You can have the command write the query output to a file using the `--output <file>` option. 

For example:

```shell
gemforge query local --json --output ./query-result.txt
```

The output of the command (without the Gemforge logging) will now be contained in JSON format in `./query-result.txt`. 
