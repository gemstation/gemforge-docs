---
icon: code
order: -7
---

# Supported frameworks

At present the following two Solidity development frameworks are supported:

* [Foundry](https://getfoundry.sh/) - Fast, rust-based. **RECOMMENDED**.
* [Hardhat](https://hardhat.org/) - Javascript/Typescript-based, more plugins.

We recommend Foundry over Hardhat because it's much faster (especially for testing) and allows for tests to be written in Solidity. 

Gemforge is built with Foundry primarily in mind. However, sample projects for both frameworks are available:

* Foundry: https://github.com/gemstation/contracts-foundry
* Hardhat: https://github.com/gemstation/contracts-hardhat

Both sample projects also demonstrate the following:

* [Custom structs](./development/custom-structs.md)
* [Source-code verification](./development/source-verification.md)

!!!
The sample projects both use Git submodules to load in essential third-party libraries.
!!!


