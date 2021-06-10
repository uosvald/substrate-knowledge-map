<!-- MarkdownTOC autolink="true" -->

- [Substrate Knowledge Map for Hackathon Participants](#substrate-knowledge-map-for-hackathon-participants)
  - [Why Substrate? \(Substrate Blockchain vs Smart Contract\)](#why-substrate-substrate-blockchain-vs-smart-contract)
  - [Navigating Our Documentations](#navigating-our-documentations)
  - [Terms Clarification](#terms-clarification)
  - [Setup Your Local Development Environment](#setup-your-local-development-environment)
  - [Using Polkadot JS App & Browser Extension](#using-polkadot-js-app--browser-extension)
  - [Substrate Runtime Development](#substrate-runtime-development)
    - [Rust](#rust)
    - [Substrate High Level Architecture](#substrate-high-level-architecture)
    - [Runtime Development](#runtime-development)
  - [Polkadot JS API \(Javascript API for building dApp\)](#polkadot-js-api-javascript-api-for-building-dapp)
  - [Smart Contract](#smart-contract)
    - [Using **ink!**](#using-ink)
    - [Using **EVM / Solidity**](#using-evm--solidity)
  - [What We Do Not Cover](#what-we-do-not-cover)

<!-- /MarkdownTOC -->

# Substrate Knowledge Map for Hackathon Participants

The following is an overview of knowledge map what a hackathon participant need to know in order to develop a non-trivial application for hackathon submission. There is a high level description on each knowledge domain and point to corresponding documentation site so you can dig deeper in that subject. Tutorials will be introduced in the middle to serve as a good break from reading and actually get your hand dirty and consolidate what you have learned.

## Why Substrate? (Substrate Blockchain vs Smart Contract)

One question we often get is why bother learning Substrate framework when we can write smart contracts.

Substrate framework and writing smart contracts are two different approaches to building "decentralized applications".

Traditional smart contract platform allows users to publish additional logic on top of some core blockchain logic. Since smart contract logic can be published by anyone, including malicious actors and inexperienced developers, there are a number of intentional safe guards and restriction built around these public smart contract platform. For example:

- Fees: Ensuring that contract users are charged for the computation and storage they force on the computers running their contract, and not allowed to abuse the block creators.
- Sandbox: A contract is not able to modify core blockchain storage or the storage of other contracts directly. It's power is limited to only modifying it's own state, and the ability to make outside calls to other contracts or runtime functions.
- Reversion: A contract can be prone to have situations which lead to logical errors. You will need additional pattern so split logic and data so you can upgrade your smart contract logic seamlessly.

These different overheads makes running contracts slower and more costly, but again, the "target audience" for contract development is different than runtime developers.

But contracts allows your community to extend and develop on top of your runtime logic without needing to go through all the craziness of proposals, runtime upgrades, etc... It may even be used as a testing grounds for future runtime changes, but done in a way that isolates your network from any of the growing pains or errors which may occur.

In summary, Substrate Smart Contracts:

- Are inherently safer to the network.
- Have built in economic incentives and transaction fee mechanism built-in not under direct control of smart contract author
- Have computational overhead to support graceful failures in logic.
- Have a lower bar to entry for development, and enable faster pace community interaction.

---

On the other hand, Substrate runtime development afford none of these protections or safe guards. As a runtime developer, you have total control on how the blockchain behave, but the bar to entry on the code you produce also jumps way up.

You have full control of the underlying logic that each node on your network will run. You have full access to each and every storage item across all of your modules, which you can modify and control.

Substrate runtime module development has the intention of producing lean, performant, and giving you the full control of the blockchain behavior. If you want, you can swap the blockchain logic to have a different consensus mechanism, have a non-conventional way of charging transaction cost (e.g. subsidy to some user groups). It provides none of the protections or overhead of transaction reverting. This mean it is up to you to correctly assess and apply fees to the different parts of your runtime logic such that it is not abused by bad actors and hurt your network.

We also have a mechanism of allowing the Substrate runtime (a.k.a. state transition function, or STF) to be upgraded with a single transaction instead of having to organize a community hard-fork. This is one of the prominent features when Substrate is being developed.

In summary, Substrate Runtime Modules:

- Provide low level access to your entire blockchain.
- Have removed the overhead of built-in safety for performance.
- Have a high bar to entry for developers.

For more detail:

- Content on the above is partially copied from [this page](https://substrate.dev/docs/en/knowledgebase/smart-contracts/overview).

## Navigating Our Documentations

We have a few documentation sites. A key in learning Substrate is knowing how to navigate through these sites and find the information you need. Sometimes it is about knowing more of a certain concept, other times it is about looking for ways to programmatically complete certain tasks.

**[substrate.dev](https://substrate.dev/)**, this is where developers learn all technical knowledge about developing their blockchain using Substrate Framework. It consists of:

- [Knowledge Base](https://substrate.dev/docs/en/): containing the foundational concepts of building blockchain using Substrate.
- [Tutorials](https://substrate.dev/en/tutorials): hand-on tutorials for developers to follow. The first 6 tutorials are teaching the fundamental for dev to complete that I recommend every Substrate learners to go through.
- [Recipes](https://substrate.dev/recipes/#): it is like O'Reilly cookbook that written on how to complete various tasks with Substrate, e.g. setting up function weight, how to use off-chain workers to fetch HTTP requests, etc. It can also be read from end-to-end for developers to learn advanced Substrate development techniques.
- [API docs](https://substrate.dev/rustdocs): Substrate API references.

All of these we features on using [Substrate Node Template](#), the minimal Substrate blockchain node, and [Substrate Front-end template](#), a front-end built with React that connect to Substrate. Developers are encouraged to start new Substrate project based on these templates.

If you need any community support, please join the [Substrate Technical matrix channel](https://app.element.io/#/room/!HzySYSaIhtyWrwiwEV:matrix.org).

**[Polkadot Wiki](#)**, this site documents the behavior and mechanism of Polkadot network, such as its governance mechanism, how transaction fee is calculated, and a section on parachains, etc. Polkadot network is built using Substrate with a lot of additional custom modules to constitute the current sophisticated behavior.

**[Polkadot JS API doc](#)**, documents on how to use Polkadot-JS API javascript API, for developers to build dApp to communicate with Substrate-based blockchain.

## Terms Clarification

- **Substrate**: the framework built for writing highly customized blockchain
- **Polkadot**: Polkadot is the mainnet that built on top of Substrate
- **Kusama**: Kusama is another network similar with Polkadot that served as the canary to launch new features before these features are also launched in Polkadot. You could view it as a beta-net except that it is also a value-bearing network and the state on the blockchain network will not be reset.
- **Web 3.0**: is the decentralized internet ecosystem that, instead of the app are centrally stored in a few servers and managed by a sovereign party, is an open, trustless, and permissionless networks.
- **Web3 Foundation**: A foundation setup to nurture cutting-edge applications for decentralized web software protocols. See more in [the website](https://web3.foundation/about/).

## Setup Your Local Development Environment

You will need to setup your local machine to run Rust and have both stable and night version of Rust installed. Because currently Substrate runtime is first compiled to native binary using stable Rust and WASM binary using nightly rust.

You can follow this **[tutorial: Create Your First Substrate Chain](https://substrate.dev/docs/en/tutorials/create-your-first-substrate-chain/)** to setup your machine local environment.

Also refer to:

- [Setup Substrate in unix-based machines](https://substrate.dev/docs/en/knowledgebase/getting-started/)
- [Setup Substrate in Windows](https://substrate.dev/docs/en/knowledgebase/getting-started/windows-users)

## Using Polkadot JS App & Browser Extension

[Polkadot JS App](http://polkadot.js.org/apps) is the canonical front-end to interact with Substrate-based chains and wallets to manage your Substrate user accounts.

Two keys to note:

1. Hosted Polkadot JS App can connect to any Substrate-based chain, not just the default Polkadot mainnet. You can config the endpoint where the front-end connected to, even to your `localhost` running node. Refer to the following two diagrams.

![assets/01-polkadot-app-endpoint.png](./assets/01-polkadot-app-top-left.png)

![assets/02-polkadot-app-select-endpoint.png](./assets/02-polkadot-app-select-endpoint.png)

2. If connecting to a custom chain (or your locally-running node), you may need to specify the custom type in JSON under *Settings* > *Developer*.

  Polkadot-JS App only receive a series of bytes from the blockchain. It is up to the user specific custom type on how to restructure back the proper data structure. Refer to:

  - [Polkadot JS doc: Type basics](https://polkadot.js.org/docs/api/start/types.basics), and
  - [Polkadot JS doc: Extending types](https://polkadot.js.org/docs/api/start/types.extend)

There is also a metamask-like browser extension to manage your Polkadot account, [Polkadot-js Extension](https://polkadot.js.org/extension/).

## Substrate Runtime Development

### Rust

You will need to know Rust fairly well to understand what's going on in Substrate and to program in Substrate framework.

Refer to [The Rust Book](https://doc.rust-lang.org/book/). You should know the materials from ch01 - ch10. There are also advanced Rust concepts required that are addressed in ch13 - 14, 18. Substrate also leverage a lot on techniques mentioned in ch19.2 - ch19.5.

### Substrate High Level Architecture

To know more about the high level architecture of Substrate. Please go through the documents on **[Getting Started: Overview](https://substrate.dev/docs/en/)** and **[Getting Started: Architecture](https://substrate.dev/docs/en/knowledgebase/getting-started/architecture)**.

In this document, we assume you will always development Substrate runtime with FRAME and node. This is what a Substrate node consists of.

![assets/03-substrate-architecture.png](./assets/03-substrate-architecture.png)

Each node has many components that manage things like the transaction queue, communicating over a P2P network, reaching consensus on the state of the blockchain, and the chain's actual runtime logic. Each aspect of the node is interesting in its own right, and the runtime is particularly interesting because it contains the business logic (aka "state transition function") that codifies the chain's functionality. The runtime contains a collection of pallets that are configured to work together.

And we will always stick to FRAME v2 syntax in this document.

On the node level, we leverage on [libp2p](#) for the p2p networking layer and put the transaction pool, consensus mechanism, and underlying data storage (a key-value database) on the node level. But note that these components are relatively advanced and we will just mention their existence but not dwell into them.

### Runtime Development

Please go through the following documents to know the fundamentals of runtime development.

- [Key Concept: Runtime](https://substrate.dev/docs/en/knowledgebase/runtime/), this is where the blockchain state transition function (the blockchain application-specific logic) is defined. It is about composing multiple pallets (can be understood as modules) together and hooking them up together.

- [Runtime Develpment: Pallets](https://substrate.dev/docs/en/knowledgebase/runtime/pallets), this doc describe what the basic structure of a Substrate module is consisted of.

- [Runtime Development: FRAME](https://substrate.dev/docs/en/knowledgebase/runtime/frame), this doc gives a high level overview on what system pallets that Substrate has already implemented for you as runtime engineer.

- [Tutorial: Add a Pallet](https://substrate.dev/docs/en/tutorials/add-a-pallet/), this tutorial goes through how to configure Substrate runtime to use a pallet.

- [Runtime Development: Storage](https://substrate.dev/docs/en/knowledgebase/runtime/storage), this doc describes how data is stored on-chain and how you could access them.

- [Runtime Development: Metadata](https://substrate.dev/docs/en/knowledgebase/runtime/metadata), this doc describes the API opened from the chain allowing external parties query what API is open, and how you could use these information when building your dApp.

- [Runtime Development: Execution](https://substrate.dev/docs/en/knowledgebase/runtime/execution), this page describe how a block is being produced, and how transactions are selected and executed to reach the next "stage" in the blockchain.

- [Runtime Development: Events](https://substrate.dev/docs/en/knowledgebase/runtime/events) & [Errors](https://substrate.dev/docs/en/knowledgebase/runtime/errors), these two pages describe how external parties know what has happened in the blockchain, via the emitted events and errors when executing transactions.

- [Tutorial: Build a PoE Decentralized Application](https://substrate.dev/docs/en/tutorials/build-a-dapp/), a good tutorial of hand-on building a dApp using Substrate from end-to-end, and building a simple front-end to interact with it.

- [Tutorial: Start a Private Network with Substrate](https://substrate.dev/docs/en/tutorials/start-a-private-network/). This tutorial helps you run a blockchain network with out-of-the-box Substrate node.

- [Off-chain Features](https://substrate.dev/docs/en/knowledgebase/learn-substrate/off-chain-features)

  There are certain limits on on-chain logic. They include that the computation cannot be too intensive that it affect the block output time, and the computation must be deterministic. This means that computation rely on external data fetching cannot be done on-chain. In Substrate, developers can also run computation off-chain and have the result send back on-chain via extrinsics. The following articles cover them.

  - [Recipes: Submit signed and unsigned transactions from off-chain workers back on-chain](https://substrate.dev/recipes/off-chain-workers/transactions.html)
  - [Recipes: Fetch external data using HTTP requests and parse JSON responses](https://substrate.dev/recipes/off-chain-workers/http-json.html)
  - [Recipes: Store result in off-chain worker local storage](https://substrate.dev/recipes/off-chain-workers/storage.html)

## Polkadot JS API (Javascript API for building dApp)

[Polkadot JS API](https://polkadot.js.org/docs/api/) is the javascript API for Substrate so you can build a javascript frontend and interact with Substrate-based blockchain.

[Substrate Front-end Template](https://github.com/substrate-developer-hub/substrate-front-end-template) is an example of using Polkadot JS API in your React front-end ([hosted version here](https://substrate.dev/substrate-front-end-template/)).

## Smart Contract

Learn about the difference between smart contract development vs Substrate runtime development, and when to use each [here](https://substrate.dev/docs/en/knowledgebase/smart-contracts).

In Substrate, you can program smart contract in either ink! for EVM (solidity language).

### Using **ink!**

- Learn about [ink! Smart Contracts](https://substrate.dev/docs/en/knowledgebase/smart-contracts/ink-fundamentals)
- Learn about [ink! Smart contract development](https://substrate.dev/docs/en/knowledgebase/smart-contracts/ink-development)
- [Tutorial: ink! Smart Contracts Tutorial](https://substrate.dev/substrate-contracts-workshop/#/)
- Mechanism on how ink! smart contract works within [Contracts pallet](https://substrate.dev/docs/en/knowledgebase/smart-contracts/contracts-pallet)

### Using **EVM / Solidity**

- Learn about using the [EVM pallet](http://localhost:3000/docs/en/knowledgebase/smart-contracts/evm-pallet).
- [Tutorial: Substrate Frontier - Configure Substrate to run EVM and accepting Solidity Contracts](https://substrate.dev/frontier-workshop)
- [Reference documentation for Solidity](https://docs.soliditylang.org)

## What We Do Not Cover

A lot :)

- On-chain runtime upgrade. We have a tutorial on [On-chain(forkless) Runtime Upgrade](https://substrate.dev/docs/en/tutorials/forkless-upgrade/). This demo introduces how to perform runtime upgrade and scheduled runtime upgrade as a chain transaction.
- About [Transaction Weight](https://substrate.dev/docs/en/knowledgebase/learn-substrate/weight) and [Fee](https://substrate.dev/docs/en/knowledgebase/runtime/fees)
- [Benchmarking your runtime to determine the transaction cost](https://substrate.dev/docs/en/knowledgebase/runtime/benchmarking)
- About [Session keys](https://substrate.dev/docs/en/knowledgebase/learn-substrate/session-keys)
- Learn more about [using pallet macro](https://substrate.dev/docs/en/knowledgebase/runtime/macros), and [the api doc](https://substrate.dev/rustdocs/v3.0.0/frame_support/attr.pallet.html).
- [Writing test for pallet functions](https://substrate.dev/docs/en/knowledgebase/runtime/tests).
- [Tightly- and Loosely-coupled pallets](https://substrate.dev/recipes/pallet-coupling.html), calling other pallet functions from one pallet via trait specification.
- [Blockchain Consensus Mechansim](https://wiki.polkadot.network/docs/en/learn-consensus), and a recipe on  customizing it to proof-of-work [here](https://substrate.dev/recipes/basic-pow.html).
- [Detail steps on account generation](https://wiki.polkadot.network/docs/en/learn-account-generation)
- **[Parachains](https://wiki.polkadot.network/docs/en/learn-parachains)**: one key feature of Polkadot is the capability of becoming a parachain. You can develop your own application-specific logic in your chain and rely on the validator community of the relay-chain to secure your network, instead of building another validator community yourself. Learn more about:

  - [Cross-chain Message Passing (XCMP)](https://wiki.polkadot.network/docs/en/learn-crosschain), how parachain and relay-chain communicate to each others
  - [Workshop: Using cumulus to build your parachain](https://substrate.dev/cumulus-workshop)
