---
order: -10
---

# Security

It's important to keep your configuration file secure by not storing sensitive information inside it. This includes but is not limited to:

* Mnemonic / seed phrases used for production deployments
* Infura / Alchemy RPC URLs which contain your API key
* Any other passwords and/or API keys

## Environment secrets

The default [configuration file template](https://github.com/gemstation/gemforge/blob/master/templates/gemforge.config.cjs) gives one example of how this can be accomplished using environment variables. For example:

```js
// gemforge.config.cjs
module.exports = {
  ...
  wallets: {
    wallet1: {
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
  },
  ...
}
```

To make this work you would have to supply the `MNEMONIC` environment variable on the command-line or in the shell environment. For example:


+++ In-line
```shell
MNEMONIC="..." gemforge deploy
```
+++ Shell environment
```shell
export MNEMONIC="..."
gemforge deploy

```
+++

## .env

Another option is to use the [dotenv](https://www.npmjs.com/package/dotenv) package within your config file to load in these environment variables from a `.env` file:

```
MNEMONIC="..."
```

Then in `gemforge.config.cjs`:

```js
require('dotenv').config();
module.exports = {
  // ... process.env.MNEMONIC will now be set
}
```

A working example of this can be seen in the [sample project](https://github.com/gemstation/contracts-foundry).

!!!
Note: Remember to add `.env` to your `.gitignore` if you use this method.
!!!

