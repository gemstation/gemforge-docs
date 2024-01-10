---
order: -5
---

# ABI generation

The [build](../commands/build.md) command will auto-generate an [ABI](https://docs.soliditylang.org/en/v0.8.21/abi-spec.html) for the Diamond. The ABI will be stored in `abi.json`, next to the [generated Diamond proxy code](../configuration/paths.md).

This ABI contains all of the functions exported in the Diamond proxy interface as well as any and all [custom errors](https://docs.soliditylang.org/en/v0.8.21/abi-spec.html#errors) and events found in _all_ the compiled artifacts (including imported libraries).

This allows front-end libraries such as such as [Wagmi](https://wagmi.sh) to recognize and decode errors and events emitted by your Diamond.

