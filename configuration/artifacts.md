---
order: -5
---

# Artifacts

Gemforge needs to know the format the artifacts are in:

```js
// gemforge.config.cjs
module.exports = {
  ...
  // artifacts configuration
  artifacts: {
    // artifact format - "foundry" or "hardhat"
    format: 'foundry',
  },  
  ...
}
```

