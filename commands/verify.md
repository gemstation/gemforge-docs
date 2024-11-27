---
order: -4
---

# verify

The `verify` command verifies the source-code of deployed contracts on Etherscan and other similar block explorers.

To run it:

```shell
gemforge verify <target>
```

This will query the deployment records to find the currently deployed contracts for the specified `<target>` which must be defined in the [targets configuration](../configuration//targets.md). 

It then goes through the list of contracts in the deployment records and proceeds to send a source-code verification call to the API of the [configured block explorer](../configuration/networks.md).

The output will look something like:

```shell
GEMFORGE: Selected target: testnet
GEMFORGE: Setting up target network connection "base_sepolia" ...
GEMFORGE:    Network chainId: 84532
GEMFORGE: Setting up wallet "wallet2" ...
GEMFORGE: Wallet deployer address: 0x82180b6EF1245318E631E18B391CD7b4a0C216B0
GEMFORGE: Load existing deployment ...
GEMFORGE:    Existing deployment found at: 0xE59350a5F03d5A29666eBA977a7122C26CDC3dB2
GEMFORGE: Checking if existing deployment is still valid...
GEMFORGE: Verifying contract DiamondProxy deployed at 0xE59350a5F03d5A29666eBA977a7122C26CDC3dB2 ...
Start verifying contract `0xE59350a5F03d5A29666eBA977a7122C26CDC3dB2` deployed on mainnet

Contract [src/generated/DiamondProxy.sol:DiamondProxy] "0xE59350a5F03d5A29666eBA977a7122C26CDC3dB2" is already verified. Skipping verification.
GEMFORGE: ...verified.
```

For more information see the [source-code verification](../development/source-verification.md) docs.

!!!
This command assumes that contract artifacts are present in the build output folder and that they match what is deployed for the given target. Thus, it's best to run this command immediately after you deploy/upgrade a Diamond.
!!!
