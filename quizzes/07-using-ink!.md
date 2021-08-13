# Quiz 7: Contracts

## Overview

This quiz covers the following parts of the Substrate Knowledge Map:

- [Smart Contract](../../knowledge-map#smart-contract)
- [Using **ink!**](../../knowledge-map#using-ink!)

### 1. What pallets and functionalities does Substrate provide to support Smart Contract execution? Check all that apply.

- [ ] `pallet-contracts`: Offers functionality for a Substrate runtime to deploy and execute WebAssembly smart contracts.
- [ ] `pallet-evm`: Allows unmodified EVM code to be executed in a Substrate-based blockchain.
- [ ] `pallet-treasury`: Provides a "pot" of funds to be deducted when running transactions on smart contracts.
- [ ] `pallet-membership`: Contains the logic for managing the access control of the smart contract on who can call which functions.

### 2. How does Substrate implement functionality for running Solidity code?

- [ ] It sends cross-chain messages over to Ethereum network.
- [ ] It embeds a SputnikVM EVM engine to run solidity ABI code.
- [ ] It implements the EVM engine specification in `pallet-contracts`.
- [ ] It creates a simulated environment for running EVM code, and puts the final computed result back to the on-chain stroage.

### 3. What is true about Smart Contracts in Substrate?

- [ ] You can deploy a contract once and instantiate it multiple times.
- [ ] When you build a contract, three files are generated: a Wasm binary, the native binary, and the metadata files.
- [ ] Contracts are intended to be used by any user on a public network. But a contract can specify certain other contracts to modify its own storage directly.
- [ ] A contract account is charged proportionally to the amount of storage its account uses. When a contract's balance goes below a defined limit, the contract's account is turned into a "tombstone" and its storage is cleaned up.
