---
order: 0
---

# init

The `init` command is used initialize a Gemforge configuration file for an existing Diamond contract project of yours. This will then allow you to run Gemforge commands against said project.

Gemforge assumes that your project uses the [diamond-2-hardhat](https://github.com/mudgen/diamond-2-hardhat) reference repository for the core Diamond Standard libraries. It also assumes that the core facets - [DiamondLoupe](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/DiamondLoupeFacet.sol), [DiamondCut](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/DiamondCutFacet.sol), [Ownership](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/OwnershipFacet.sol) - are part of your diamond.

To create the default config file:

+++ Foundry
```shell
gemforge init
```
+++ Hardhat
```shell
gemforge init --hardhat
```
+++

This will create a file in the current folder called `gemforge.config.cjs`. You can edit this file to [customize the configuration](../configuration).

To customize the name of the config file, use the `--name` option:

```shell
gemforge init --name <custom_file_name>
```

!!!
Note: It's important to keep the `.cjs` file extension if customizing the config file name, as this is required for Gemforge to be able to load it correctly.
!!!

