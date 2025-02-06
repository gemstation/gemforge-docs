---
order: -8
---

# Multisig upgrades

Malicious upgrades are a very real attack vector for Diamond proxies, and especially so if the Diamond owner is a hot wallet.

For this reason it's good practice to set the owner of any upgradeable contract to be a [Multisig (multi-signature)](https://www.ledger.com/academy/what-is-a-multisig-wallet) or [MPC (multi-party computation)](https://www.coinbase.com/en-sg/learn/wallet/what-is-a-multi-party-computation-mpc-wallet) wallet so that upgrades are protected by multiple authorizations.

Once done you will need to tell Gemforge to output the upgrade transaction data so that you can send the transaction manually from within your Multisig/MPC wallet.

To do this set the [target upgrades configuration](../configuration/targets.md) as follows:

```js
  targets: {
    // Target named "main"
    main: {
      upgrades: {
        // Whether the diamondCut() call will be done manually.
        manualCut: true
      }
    }
  }
```

By setting `manualCut` to `true` Gemforge will not call `diamondCut()` but will instead output the raw transaction data so that you can call it yourself. It will look something like this in the console output:

```shell
GEMFORGE: Outputting upgrade tx params and skipping actual tx call...

GEMFORGE: ================================================================================

GEMFORGE: Diamond: 0x3351cb6fD12655854DDAa85b0D9857d3e20a1310

GEMFORGE: Tx data: 0x1f931c1c0000000...00000000000000000

GEMFORGE: ================================================================================
```

You can then copy-paste this information into your Multisig/MPC wallet's transaction builder to actually sign and send the transaction to finish the upgrade.


!!!
Adding multi-sig wallet support natively to Gemforge is [currently being proposed](https://github.com/gemstation/gemforge/issues/11).
!!!