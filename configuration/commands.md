---
order: -2
---

# Commands

Gemforge executes certain commands via the underlying [development framework](../frameworks.md). 

These must be configured as follows:


```js
// gemforge.config.cjs
module.exports = {
  ...
  commands: {
    // the build command
    build: 'forge build', // replace with your own custom build/compilation command
  },  ...
}
```

