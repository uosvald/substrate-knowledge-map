# Quiz 6: Polkadot JS API

## Overview

This quiz covers the following parts of the Substrate Knowledge Map:

- [Polkadot JS API](../../knowledge-map#polkadot-js-api)

### 1. What are the differences between Polkadot-JS Apps and Polkadot-JS API? Check all that apply.

- [ ] Polkadot-JS Apps provides a front-end UI and wallet to interact with Substrate-based networks.
- [ ] Polkadot-JS API is the official JS API that connects to Substrate-based networks.
- [ ] Polkadot-JS API is the official front-end UI to interact with any Substrate-based chain.
- [ ] Polkadot-JS API, in addition to Substrate-based networks, can also connect to other mainstream blockchains such as Bitcoin and Ethereum.

### 2. In Polkadot-JS API, what datatype is used to represent a number (i.e. u64, i64, u128, i128) on a Substrate network?

- [ ] The default data type Number in javascript.
- [ ] String
- [ ] [bn.js](https://github.com/indutny/bn.js/)
- [ ] [bignumber.js](https://github.com/MikeMcl/bignumber.js/)
- [ ] [decimal.js](https://github.com/MikeMcl/decimal.js/)

### 3. What is the correct way to query a storage value in a Substrate network and subscribe to its changes?

- [ ] `const val = await api.query.my_pallet.storage`
- [ ] `const unsub = await api.query.my_pallet.storage(value => {...})`
- [ ] `const val = await api.consts.my_pallet.const`
- [ ] `const val = await api.tx.my_pallet.tx().signAndSend()`
- [ ] `const val = await api.tx.my_pallet.tx().signAndSend(value => {...})`

### 4. Assuming that you don't need to subscribe to the on-chain transaction status, what is the proper way to send an extrinsic to a Substrate network?

- [ ] `const val = await api.query.my_pallet.storage`
- [ ] `const unsub = await api.query.my_pallet.storage(value => {...})`
- [ ] `const val = await api.consts.my_pallet.const`
- [ ] `const val = await api.tx.my_pallet.tx().signAndSend()`
- [ ] `const val = await api.tx.my_pallet.tx().signAndSend(value => {...})`
