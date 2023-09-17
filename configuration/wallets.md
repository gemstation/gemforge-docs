---
order: -8
---

# Wallets

Wallets are required for deploying to a [target](./targets.md).

They are configured as follows:

```js
// gemforge.config.cjs
module.exports = {
  ...
  wallets: {
    // Wallet named "wallet1"
    wallet1: {
      // Wallet type - mnemonic
      type: 'mnemonic',
      // Wallet config
      config: {
        // Mnemonic phrase
        words: 'your menomonic phrase here',
        // 0-based index of the account to use
        index: 0,
      }
    },
    wallet2: {
      // Wallet type - mnemonic
      type: 'mnemonic',
      // Wallet config
      config: {
        // Mnemonic phrase
        words: () => process.env.MNEMONIC,
        // 0-based index of the account to use
        index: 0,
      },
    },
    // ...
    // add more wallets here
    // ...
  },
  ...
}
```

The `words` config parameter for `mnemonic` wallets can either be a string of words or a method which returns the same.

!!!
Note: At present only the `mnemonic` wallet type is supported. We aim to add more wallet types in future.
!!!