<!-- MarkdownTOC autolink="true" -->

- [Substrate Knowledge Map for Hackathon Participants](#substrate-knowledge-map-for-hackathon-participants)
  - [Why Substrate? \(Substrate Blockchain vs Smart Contract\)](#why-substrate-substrate-blockchain-vs-smart-contract)
  - [Navigating Documentation Sites](#navigating-documentation-sites)
  - [Term Clarification: Substrate, Polkadot/Kusama, Web 3.0, etc](#term-clarification-substrate-polkadotkusama-web-30-etc)
  - [Setup Your Local Development Environment](#setup-your-local-development-environment)
  - [Using Polkadot JS App & Browser Extension](#using-polkadot-js-app--browser-extension)
  - [Developing on Substrate Runtime](#developing-on-substrate-runtime)
    - [Know Some Rust](#know-some-rust)
    - [Substrate High Level Architecture](#substrate-high-level-architecture)
    - [Pallet Development](#pallet-development)
    - [Pallet Storage](#pallet-storage)
    - [Pallet Runtime logic](#pallet-runtime-logic)
    - [Pallet Events & Errors](#pallet-events--errors)
    - [Tutorial: Build a PoE Decentralized Application](#tutorial-build-a-poe-decentralized-application)
    - [Configuring Your Substrate Runtime to Use a Pallet](#configuring-your-substrate-runtime-to-use-a-pallet)
    - [Tutorial: Add a Pallet to Your Runtime](#tutorial-add-a-pallet-to-your-runtime)
  - [Polkadot JS API \(Javascript API for building dApp\)](#polkadot-js-api-javascript-api-for-building-dapp)
    - [Working with Substrate Front-end Template](#working-with-substrate-front-end-template)
  - [Others](#others)
    - [Tutorial: Start a Private Network with Substrate](#tutorial-start-a-private-network-with-substrate)
    - [Off-chain Workers](#off-chain-workers)
  - [Building using Smart Contract](#building-using-smart-contract)
    - [ink!](#ink)
    - [Using EVM](#using-evm)
  - [Advanced Domains in Substrate](#advanced-domains-in-substrate)
    - [On-chain\(forkless\) Runtime Upgrade](#on-chainforkless-runtime-upgrade)
    - [Becoming a Parachain & Cumulus](#becoming-a-parachain--cumulus)
  - [What We Do Not Cover](#what-we-do-not-cover)

<!-- /MarkdownTOC -->


# Substrate Knowledge Map for Hackathon Participants

The following is an overview of knowledge map what a hackathon participant need to know in order to develop a non-trivial application for hackathon submission. There is a high level description on each knowledge domain and point to corresponding documentation site so you can dig deeper in that subject. Tutorials will be introduced in the middle as they serve as a good break from reading and actually get your hand dirty and consolidate what you have just learned.

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

## Navigating Documentation Sites

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

## Term Clarification: Substrate, Polkadot/Kusama, Web 3.0, etc

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

## Developing on Substrate Runtime

### Know Some Rust

You will need to know Rust fairly well to understand what's going on in Substrate and to program in Substrate framework.

Refer to [The Rust Book](https://doc.rust-lang.org/book/). You should know the materials from ch01 - ch10. There are also advanced Rust concepts required that are addressed in ch13 - 14, 18. Substrate also leverage a lot on techniques mentioned in ch19.2 - ch19.5.

### Substrate High Level Architecture

To know more about the high level architecture of Substrate. Please go through the documents on **[Getting Started: Overview](https://substrate.dev/docs/en/)** and **[Getting Started: Architecture](https://substrate.dev/docs/en/knowledgebase/getting-started/architecture)**.

In this document, we assume you will always development Substrate runtime with FRAME and node. This is what a Substrate node consists of.

![assets/03-substrate-architecture.png](./assets/03-substrate-architecture.png)

Each node has many components that manage things like the transaction queue, communicating over a P2P network, reaching consensus on the state of the blockchain, and the chain's actual runtime logic. Each aspect of the node is interesting in its own right, and the runtime is particularly interesting because it contains the business logic (aka "state transition function") that codifies the chain's functionality. The runtime contains a collection of pallets that are configured to work together.

And we will always stick to FRAME v2 syntax in this document.

On the node level, we leverage on [libp2p](#) for the p2p networking layer and put the transaction pool, consensus mechanism, and underlying data storage (a key-value database) on the node level. But note that these components are relatively advanced and we will only need to know their existence but not dwell into them.

### Pallet Development

Go through the following documents to know the fundamentals of runtime development.

- [Runtime: Overview]()
- [Runtime: Primitives]()
- [Runtime: FRAME]()
- [Runtime: Pallets]()

### Pallet Storage

### Pallet Runtime logic

### Pallet Events & Errors

### Tutorial: Build a PoE Decentralized Application

====>>>>====>>>>====>>>>====>>>>

### Configuring Your Substrate Runtime to Use a Pallet

### Tutorial: Add a Pallet to Your Runtime

## Polkadot JS API (Javascript API for building dApp)

### Working with Substrate Front-end Template

## Others

### Tutorial: Start a Private Network with Substrate

### Off-chain Workers

#### Sending transactions back on-chain

#### Sending HTTP Requests & JSON Parsing

#### Off-chain Storage


---
## Building using Smart Contract

### ink!

#### Tutorial: ink! Smart Contracts Tutorial

### Using EVM

#### Tutorial: Configure Substrate to run EVM and accepting Solidity Contracts

---

## Advanced Domains in Substrate

### On-chain(forkless) Runtime Upgrade

### Becoming a Parachain & Cumulus

---

## What We Do Not Cover

A lot :)
