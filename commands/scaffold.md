---
order: -1
---

# scaffold

The `scaffold` command is used to setup a minimal demo project with Gemforge, as a starting point for one's own project. 

This saves you time since everything will already be setup for you to start building with.

To setup the scaffolding in the current folder:

+++ Foundry
```shell
gemforge scaffold 
```
+++ Hardhat
```shell
gemforge scaffold --hardhat
```
+++

To setup the scaffolding in a specific folder, use the `--folder` option:

+++ Foundry
```shell
gemforge scaffold --folder /path/to/folder
```
+++ Hardhat
```shell
gemforge scaffold --folder /path/to/folder --hardhat
```
+++


!!!
Note: Gemforge will complain if the target folder isn't empty. If it doesn't exist it will be created automatically by Gemforge.
!!!
