<!-- MarkdownTOC autolink="true" -->

- [Substrate Knowledge Map for Hackathon Participants](#substrate-knowledge-map-for-hackathon-participants)
  - [Why Substrate? \(Substrate Blockchain vs Smart Contract\)](#why-substrate-substrate-blockchain-vs-smart-contract)
  - [Navigating Our Documentations](#navigating-our-documentations)
  - [Setup Your Local Development Environment](#setup-your-local-development-environment)
  - [Interact with a Substrate Network \(using Polkadot-JS Apps\)](#interact-with-a-substrate-network-using-polkadot-js-apps)
  - [Substrate Runtime Development](#substrate-runtime-development)
    - [Rust](#rust)
    - [Substrate High Level Architecture](#substrate-high-level-architecture)
    - [Runtime Development](#runtime-development)
  - [Polkadot JS API \(Javascript API for building dApp\)](#polkadot-js-api-javascript-api-for-building-dapp)
  - [Smart Contract](#smart-contract)
    - [Using **ink!**](#using-ink)
  - [What We Do Not Cover](#what-we-do-not-cover)
  - [Terms Clarification](#terms-clarification)

<!-- /MarkdownTOC -->

# Substrate Knowledge Map for Hackathon Participants

The following is an overview of knowledge map what a hackathon participant need to know in order to develop a non-trivial application for hackathon submission. There is a high level description on each domain and point to the corresponding documentation so you can dig deeper in that subject. Tutorials will be introduced in the middle to serve as a good break from reading and actually get your hand dirty and consolidate what you have learned.

## Why Substrate? (Substrate Blockchain vs Smart Contract)

One question we often get is why learning Substrate framework when we can write smart contracts.

Substrate framework and writing smart contracts are two different approaches to building "decentralized applications".

Traditional smart contract platform allows users to publish additional logic on top of some core blockchain logic. Since smart contract logic can be published by anyone, including malicious actors and inexperienced developers, there are a number of intentional safe guards and restriction built around these public smart contract platform. For example:

- **Fees**: Ensuring that contract users are charged for the computation and storage they force on the computers running their contract, and not allowed to abuse the block creators.

- **Sandboxed**: A contract is not able to modify core blockchain storage or the storage of other contracts directly. It's power is limited to only modifying it's own state, and the ability to make outside calls to other contracts or runtime functions.

- **Reversion**: A contract can be prone to have situations which lead to logical errors. You will need additional pattern such as splitting logic and data so you can upgrade your smart contract logic seamlessly.

These different overheads makes running contracts slower and more costly, but again, the "target audience" for contract development is different than runtime developers.

But contracts allows your community to extend and develop on top of your runtime logic without needing to go through all the craziness of proposals, runtime upgrades, etc... It may even be used as a testing grounds for future runtime changes, but done in a way that isolates your network from any of the growing pains or errors which may occur.

In summary, smart contracts:

- Are inherently safer to the network.
- Have built in economic incentives and transaction fee mechanism built-in not under direct control of smart contract author.
- Have computational overhead to support graceful failures in logic.
- Have a lower bar to entry for development, and enable faster pace of community interaction.

---

On the other hand, Substrate runtime development afford none of these protections or safe guards. As a runtime developer, you have total control on how the blockchain behave, but the bar to entry on the code you produce also jumps way up.

You have full control of the underlying logic that each node on your network will run. You have full access to each and every storage item across all of your modules, which you can modify and control.

We also have a mechanism of allowing the Substrate runtime (a.k.a. state transition function, or STF) to be upgraded with a single transaction instead of having to organize a community hard-fork. This is one of the prominent features when Substrate is being developed.

In summary, runtime development:

- Provide low level access to your entire blockchain.
- Have removed the overhead of built-in safety for performance.
- Have a high bar to entry for developers.

For more detail, refer to [**Smart Contract - Overview**](https://substrate.dev/docs/en/knowledgebase/smart-contracts/overview). Content on the above partially came from this resources.

## Navigating Our Documentations

We have a few documentation sites. One key in learning Substrate is knowing how to navigate through these sites and find the information you need. Sometimes the objective is to learn more about a certain concept, other times it is to look for code to perform certain tasks.

[**Substrate Developer Hub**](https://substrate.dev/), this site has the most comprehensive all-round coverage about Substrate, from big picture architecture explanation to various technical concepts. It also contains series of tutorials covering a wide breadth of topics, and finally the API docs. This should be the first go-to place if you want to look up a certain aspect during Substrate runtime development. The site is consisted of:

- [Knowledge Base](https://substrate.dev/docs/en/): Explaning the foundational concepts of building blockchain runtime using Substrate.

- [Tutorials](https://substrate.dev/en/tutorials): Hand-on tutorials for developers to follow. **The first SIX tutorials are showing the fundamentals in Substrate and are recommended for every Substrate learner to go through.**

- [Recipes](https://substrate.dev/recipes) / [How-to Guides](https://substrate.dev/substrate-how-to-guides/): It is like the O'Reilly cookbook series written in a task-oriented way for readers to get the job done, e.g. setting up proper weight functions for extrinsics, using off-chain workers to fetch HTTP requests, etc. It can also be read from cover-to-cover for developers to learn advanced Substrate development techniques. We are slowly migrating content over from Recipes to How-to Guides.

- [API docs](https://substrate.dev/rustdocs): Substrate API references.

We also feature a light weight Substrate node [Substrate Node Template](https://github.com/substrate-developer-hub/substrate-node-template), the minimal Substrate blockchain node, and [Substrate Front-end template](https://github.com/substrate-developer-hub/substrate-front-end-template), a front-end built with React using [Polkadot-JS API](https://polkadot.js.org/docs/api/) to connect to Substrate. Developers are encouraged to start new Substrate project based on these templates.

If you face that technical questions and need support, feel free to join the [Substrate Technical matrix channel](https://app.element.io/#/room/!HzySYSaIhtyWrwiwEV:matrix.org).

[**Polkadot Wiki**](https://wiki.polkadot.network/), this site documents the specific behavior and mechanism of Polkadot network, which is built using Substrate framework. Having said that, Polkadot has a lot of customization/specialization built on top of Substrate, in particular, acting as a relayer for multiple blockchains to connect in, and allow them to have cross-blockchain message passing. If you want to know more how that is done, please check the wiki.

[**Polkadot JS API doc**](https://polkadot.js.org/docs/api/), documents on how to use Polkadot-JS API, a set of javascript API allowing developers having their dApps or frontend connect to Substrate-based blockchains to query and send transactions over.

## Setup Your Local Development Environment

Here You will setup your local machine to install Rust compiler and have both stable and nightly version of the compiler installed. This is because currently Substrate runtime is compiled to native binary using the stable Rust compiler, and then to WASM binary, which only nightly Rust compiler could do this.

👉 Please follow this [**Tutorial: Create Your First Substrate Chain**](https://substrate.dev/docs/en/tutorials/create-your-first-substrate-chain/) to setup your local development environment.

Also refer to:

- [Setup Substrate in unix-based machines](https://substrate.dev/docs/en/knowledgebase/getting-started/)
- [Setup Substrate in Windows](https://substrate.dev/docs/en/knowledgebase/getting-started/windows-users)

## Interact with a Substrate Network (using Polkadot-JS Apps)

[Polkadot JS Apps](http://polkadot.js.org/apps) is the canonical front-end to interact with Substrate-based chains and wallets to manage your Substrate user accounts.

Two keys to note:

1. Polkadot-JS Apps can be used to connect to any remote Substrate-based chain, not just the default Polkadot mainnet. You can config the endpoint where Apps connected to, even to your `localhost` running node. Refer to the following two diagrams.

Click on the top left side showing your currently connected network:

![assets/01-polkadot-app-endpoint.png](./assets/01-polkadot-app-top-left.png)

Scroll to the bottom of the menu, open **DEVELOPMENT**, and choose either **Local Node** or **Custom** to specify your own endpoint.

![assets/02-polkadot-app-select-endpoint.png](./assets/02-polkadot-app-select-endpoint.png)

👉 **Lab**: Run the Substrate Node Template locally, and then goto Polkadot-JS Apps and connect to your local node. Try the following operations:

  - Send some tokens from Alice to Bob
  - Inspect the chain state of the Template pallet, in **Developer** > **Chain State**
  - Send a transaction (aka extrinsic) to the chain, in **Developer** > **Extrinsics**,
    - Setting the module to `Template`, extrinsic `do_something`, and a number of `10`,
    - Click `Sign and Submit`

  - You should see the transaction is being submitted. After a while, on-chain events will be emitted show the transaction is processed successfully.

  - Goto **Developer** > **Chain State** again, and inspect that the module `Template` and storage `Something` has indeed been updated to the value you just submitted.

> **Notes**:
> If you are connecting Apps to a custom chain (or your locally-running node), you may need to specify the chain custom data type in JSON under **Settings** > **Developer**.
>
> Polkadot-JS Apps only receive a series of bytes from the blockchain. It is up to the developer to tell the Apps how to decode and interpret these custom data type. Refer to:
>
>  - [Polkadot JS doc: Type basics](https://polkadot.js.org/docs/api/start/types.basics)
>  - [Polkadot JS doc: Extending types](https://polkadot.js.org/docs/api/start/types.extend)

We also have [Polkadot-js Extension](https://polkadot.js.org/extension/), a metamask-like browser extension to manage your Substrate accounts.

## Substrate Runtime Development

### Rust

You will need to know Rust fairly well to understand what's going on in Substrate and to program in Substrate framework.

If you are new to Rust, or need a brush up on your Rust knowledge, please refer to [The Rust Book](https://doc.rust-lang.org/book/).
  - Read about ch1 - 10: these chapters cover the foundational knowledge of programming in Rust
  - Read about ch13 on iterators and closures
  - Read about ch18 - 19 on advanced traits, advanced types. Know a bit about macro. You will not necessarily writing your own macro, but will be using a lot of Substrate built-in macro to write your runtime.

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

## Terms Clarification

- **Substrate**: the blockchain development framework built for writing highly customized blockchain
- **Polkadot**: Polkadot is the mainnet that built on top of Substrate
- **Kusama**: Kusama is another network similar with Polkadot that served as the canary to launch new features before these features are also launched in Polkadot. You could view it as a beta-net except that it is also a value-bearing network and the state on the blockchain network will not be reset.
- **Web 3.0**: is the decentralized internet ecosystem that, instead of the app are centrally stored in a few servers and managed by a sovereign party, is an open, trustless, and permissionless networks.
- **Web3 Foundation**: A foundation setup to nurture cutting-edge applications for decentralized web software protocols. See more in [the website](https://web3.foundation/about/).

