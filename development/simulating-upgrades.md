---
order: -7
---

# Simulating upgrades

When doing upgrades in production (e.g on Mainnet) it would be prudent to first simulate the upgrade prior to running the live operation.

Although Gemforge allows for ["dry" deployments](../commands/deploy.md) these simply list what actions Gemforge would take during a real deployment. To actually understand what changes would be made to the chain you need to fork the chain locally and run the deployment command against this local copy.

Thankfully, forking a chain to test against locally is easily accomplished _([Forge docs](https://book.getfoundry.sh/tutorials/forking-mainnet-with-cast-anvil), [Hardhat docs](https://hardhat.org/hardhat-network/docs/guides/forking-other-networks))_.

## Example: Simulating a Mainnet upgrade

This example assumes the following:

* You've previously deployed your Diamond to Mainnet using Gemforge and that the contract address is saved in [`gemforge.deployments.json`](../commands/deploy.md).
* You are using [Alchemy](https://www.alchemy.com/) and have your Alchemy API key set in the shell environment as `ALCHEMY_API_KEY` _(The example works equally well with Infura and/or other node services)_.

Let's say you have your Mainnet deployment target configured in Gemforge as follows:

```js
// gemforge.config.cjs
module.exports = {
  ...
  networks: {
    mainnet: {
      rpcUrl: `https://eth-mainnet.g.alchemy.com/v2/${process.env.ALCHEMY_API_KEY}`,
    },
  },
  targets: {
    mainnet: {
      network: 'mainnet',
      wallet: 'wallet1',
      initArgs: []
    }
  }
  ...
}
```

Add another entry for the local Mainnet fork using the same config parameters:

```js
networks: {
  mainnet: /*...*/,
  mainnetFork: {
    rpcUrl: `http://localhost:8545/`,
  }
},
targets: {
  mainnet: /*..*/,
  mainnetFork: {
    network: 'mainnetFork',
    wallet: 'wallet1',
    initArgs: []
  }
}
```

Now edit the  `gemforge.deployments.json` file and duplicate the `mainnet` target entries for `mainnetFork`:

```js
// File: gemforge.deployments.json
{
  "mainnet": {
    "chainId": 1,
    "contracts": /*...*/,
  },
  "mainnetFork": {
    // Should be a copy of everything under the "mainnet" key above
  }
}
```

Now start a local fork of Mainnet:

+++ Forge
```shell
anvil --fork-url https://eth-mainnet.g.alchemy.com/v2/$ALCHEMY_API_KEY
```
+++ Hardhat
```shell
npx hardhat node --fork https://eth-mainnet.g.alchemy.com/v2/$ALCHEMY_API_KEY
```
+++

Now you can run the upgrade against the local fork:

```shell
gemforge deploy mainnetFork
```

That's it! You've just run simulated your Mainnet upgrade locally. You can inspect the locally running fork to see what changes the upgrade made.

