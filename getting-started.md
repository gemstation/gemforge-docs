---
order: -2
---

# Getting started

## Step 1 - Installation

Gemforge is written in [Node.js](http://nodejs.org) and works on all the platforms that Node runs on. 

!!!
**Node v16 or above is required.** We recommend using [nvm](https://github.com/nvm-sh/nvm) to manage different Node versions on your machine.
!!!

It's best to install Gemforge globally in your Node environment, depending on which package manager you use:

+++ pnpm
```shell
pnpm add --global gemforge
```
+++ npm
```shell
npm install --global gemforge
```
+++ yarn
```shell
yarn add --global gemforge
```
+++

At this point you should be able to run the `gemforge` command:

```shell
> gemforge --help
Usage: gemforge [options] [command]

Options:
  -V, --version               output the version number
  -h, --help                  display help for command

Commands:
  init [options]              Initialize a gemforge config file for an existing project.
  scaffold [options]          Generate diamond smart contract project scaffolding.
  build [options]             Build a project.
  deploy [options] [network]  Deploy the diamond to a network.
  help [command]              display help for command
```

## Step 2 - Project setup

==- Create a new project

Gemforge provides example projects for both **LINK: Hardhat** and **LINK: Foundry**.

These can be cloned and setup in an empty folder using the [scaffold](commands/scaffold.md) command:

+++ Foundry
```shell
gemforge scaffold --folder /path/to/new_or_empty_folder
```
+++ Hardhat
```shell
gemforge scaffold --hardhat --folder /path/to/new_or_empty_folder
```
+++

==-

==- Use with an existing project

If you have an existing Diamond Standard project then you can use the [init](commands/init.md) command to create a Gemforge [configuration file](configuration).

+++ Foundry
```shell
gemforge init
```
+++ Hardhat
```shell
gemforge init --hardhat
```
+++

The `gemforge.config.cjs` file will have been created in the current folder. You will probably need to edit this file to [customize it](configuration) for your project. 

!!!
Note: Gemforge assumes that your project uses the [diamond-2-hardhat](https://github.com/mudgen/diamond-2-hardhat) reference repository for the core Diamond Standard libraries. It also assumes that the core facets - [DiamondLoupe](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/DiamondLoupeFacet.sol), [DiamondCut](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/DiamondCutFacet.sol), [Ownership](https://github.com/mudgen/diamond-2-hardhat/blob/main/contracts/facets/OwnershipFacet.sol) - are part of your diamond.
!!!

===

## Step 3 - Build contracts

To generate all necessary Diamond code and compile your project inside the project folder:

```shell
gemforge build
```


## Step 4 - Deploy contracts

Assuming a local test node is running at http://localhost:8545, use the following to deploy your Diamond code and facets to it: 

```shell
gemforge deploy
```

At this point you've just built and deployed a Diamond Standard project to your local test node using Gemforge!






