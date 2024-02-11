---
order: -6
---

# Omni-chain addresses

Gemforge uses [CREATE3 keyless deployment](https://ethereum-magicians.org/t/keyless-contract-deployment-with-create3/16025) under the hood to deploy the Diamond proxy contract.

This makes it possible to deploy the Diamond proxy to the _same address on every chain_, as long as you are using the same wallet and `CREATE3` salt to deploy each [target](../configuration/targets.md).

As an example, let's say we are deploying to two targets - `mainnet` and `base`. The target configurations will look something like:

```js
// gemforge.config.cjs

const WALLET = 'wallet1'
const SALT =
"0xf8aac9c60a8577e3e439a5639f65f9eca367e2c6de7086f4b4076c0a895d1902";

module.exports = {
  ...
  wallets: {
    [WALLET]: {...}
  },
  ...
  targets: {
    mainnet: {
      // Ethereum Mainnet chain
      network: 'mainnet', 
      wallet: WALLET,
      initArgs: []
      create3Salt: SALT,
    },
    base: {
      // BASE chain
      network: 'base', 
      wallet: WALLET, // same wallet
      initArgs: [],
      create3Salt: SALT, // same salt
    }
  }
  ...
}
```

If we do fresh deployments to both of these targets the Diamond proxy should have the same contract address on both, regardless of the difference in `wallet1`'s activity on each chain.

!!!
If you attempt to do a fresh deployment on a chain which already has a deployment using the same wallet and salt then Gemforge will complain that the address is already in use and that you should change the salt value.
!!!

## CREATE3 factory

Gemforge uses the `CREATE3` factory from the [SKYBIT-Keyless-Deployment](https://github.com/SKYBITDev3/SKYBIT-Keyless-Deployment) repository.

Here are the details:

* The deployed address of the factory on every chain: `0x24fCFA23F3b22c15070480766E3fE2fad3E813EA`
* The deploying wallet's public key: `0xc7c0A9dc9c997438eE834bb155dF2AF7fDAe6073`
* Deployment gas limit: `350,000`
* Deployment gas price: `100 gwei` (= `100000000000 wei` or `0.0000001 eth`)
* Funds required to deploy: `0.035 eth` (= gas limit * gas price)

You can deploy the factory yourself on a new chain please ensure there are enough funds in the deploying wallet and then send the following raw signed transaction to the chain:

```
0xf903f68085174876e800830557308080b903a3608060405234801561000f575f80fd5b506103868061001d5f395ff3fe608060405260043610610028575f3560e01c806350f1c4641461002c578063cdcb760a14610074575b5f80fd5b348015610037575f80fd5b5061004b61004636600461020e565b610087565b60405173ffffffffffffffffffffffffffffffffffffffff909116815260200160405180910390f35b61004b61008236600461027d565b6100ea565b6040517fffffffffffffffffffffffffffffffffffffffff000000000000000000000000606084901b166020820152603481018290525f906054016040516020818303038152906040528051906020012091506100e382610147565b9392505050565b6040517fffffffffffffffffffffffffffffffffffffffff0000000000000000000000003360601b166020820152603481018390525f906054016040516020818303038152906040528051906020012092506100e383833461019c565b5f604051305f5260ff600b53826020527f21c35dbe1b344a2488cf3321d6ce542f8e9f305544ff09e4993a62319a497c1f6040526055600b20601452806040525061d6945f52600160345350506017601e2090565b5f6f67363d3d37363d34f03d5260086018f35f52836010805ff5806101c85763301164255f526004601cfd5b8060145261d6945f5260016034536017601e2091505f8085516020870186855af16101fa576319b991a85f526004601cfd5b50803b6100e3576319b991a85f526004601cfd5b5f806040838503121561021f575f80fd5b823573ffffffffffffffffffffffffffffffffffffffff81168114610242575f80fd5b946020939093013593505050565b7f4e487b71000000000000000000000000000000000000000000000000000000005f52604160045260245ffd5b5f806040838503121561028e575f80fd5b82359150602083013567ffffffffffffffff808211156102ac575f80fd5b818501915085601f8301126102bf575f80fd5b8135818111156102d1576102d1610250565b604051601f82017fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe0908116603f0116810190838211818310171561031757610317610250565b8160405282815288602084870101111561032f575f80fd5b826020860160208301375f602084830101528095505050505050925092905056fea2646970667358221220992118230e4c9ffed4926da567e9fee8d8a102c65d41aa5ee3579d36ca97124164736f6c634300081500331ba03333333333333333333333333333333333333333333333333333333333333333a03333333333333333333333333333333333333333333333333333333333333333
```

The ABI of the deployed factory is:

```solidity
/** Deploy a contract using the given salt value, with msg.sender as the deployer. */
function deploy(bytes32 salt, bytes memory creationCode) external payable returns (address deployed);
/** Calcualate the address at which the following deployer wallet and salt value would deploy a contract. */
function getDeployed(address deployer, bytes32 salt) external view returns (address deployed);
```
