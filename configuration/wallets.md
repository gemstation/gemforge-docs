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
      // Wallet type - private key
      type: 'private-key',
      // Wallet config
      config: {
        // Private key
        key: '0x....',
      },
    },
    // ...
    // add more wallets here
    // ...
  },
  ...
}
```

The `words` config parameter for `mnemonic` wallets can either be a string of words or a method which returns the same, e.g:

```js
config: {
  words: () => 'my mnemonic ...'
}
```

The same applies to the `key` config parameter for `private-key` wallets:

```js
config: {
  key: () => '0x...'
}
```
