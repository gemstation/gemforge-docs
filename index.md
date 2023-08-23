---
order: 0
---

![](static/logo.png)

# Introduction

Gemforge is an open-source command-line tool for building, deploying and upgrading [Diamond Standard](https://eips.ethereum.org/EIPS/eip-2535) contracts on EVM chains.

## Why

The Diamond Standard (EIP-2535) is one of the best ways to build and deploy infinite sized, upgradeable contracts.

But utilizing the standard involves having to write a lot of boilerplate code, including but not limited to the core diamond proxy contract, interface code to enable easy access for dapps, deployment code which calculates what facets to add and remove in each upgrade, etc.

**Gemforge** to the rescue!

By automating almost all aspects of this boilerplate code whilst still remaining highly configurable, Gemforge lessens the workload and saves time when developing with Diamond Standard.

!!!
Gemforge only supports Solidity. Other EVM languages are not currently supported.
!!!

