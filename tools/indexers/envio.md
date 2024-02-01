---
description: Feature-rich indexing solution and data infrastructure provider for fast and flexible access to real-time and historical data for any EVM.
---

# Envio

[Envio](https://envio.dev/) is a feature-rich indexing solution that provides developers with a seamless and efficient way to index and aggregate real-time or historical blockchain data for any EVM. The indexed data is easily accessible through custom GraphQL queries, providing developers with the flexibility and power to retrieve specific information.

Envio offers native support for Metis (both testnet and mainnet) and has been designed to support high-throughput blockchain applications that rely on real-time data for their business requirements.

Designed to optimize the user experience, Envio offers automatic code generation, flexible language support, quickstart templates, and a reliable cost-effective [hosted service](https://docs.envio.dev/docs/hosted-service).

Indexers on Envio can be written in JavaScript, TypeScript, or ReScript.

## Envio HyperSync 

Envio supports [HyperSync](https://docs.envio.dev/docs/hypersync) on Metis mainnet. 

HyperSync is an accelerated data query layer for the Metis blockchain, providing APIs that bypasses JSON-RPC for 20-100x faster syncing of historical data. HyperSync is used by default in Envio's indexing framework, with the use of RPC being optional. Using Hypersync, application developers do not need to worry about RPC URLs, rate-limiting, or managing infrastructure, and can easily sync large datasets in a few minutes, something that would usually hours or days using RPC. 

HyperSync is also available as a standalone API for data analytic use cases. Data analysts can interact with the HyperSync API using Javascript, Python or Rust client - and extract data in JSON, Arrow or Parquet formats. For more information, visit the HyperSync documentation [here](https://docs.envio.dev/docs/overview-hypersync).


## Key Features 

- Contract Import: Autogenerate the key boilerplate for an entire Indexer project off a single or multiple smart contracts. Deploy within minutes. 

- Multi-chain Support: Aggregate data across multiple networks into a single database. Query all your data with a unified GraphQL API. 

- Asynchronous Mode: Fetching data from off-chain storage such as IPFS, or contract state (e.g. smart contract view functions).

- Quickstart templates with pre-defined indexing logic for popular OpenZeppelin contracts (e.g. ERC-20).


## Getting Started

[**Quickstart Guide**](https://docs.envio.dev/docs/quickstart)

The following files are required from the user to run the Envio indexer:

- Configuration (defaults to `config.yaml`)
- GraphQL Schema (defaults to `schema.graphql`)
- Event Handlers (defaults to `src/EventHandlers.*` depending on the language chosen)

These files are auto-generated according to the template and language chosen by running the `envio init` command. 

Users can choose whether they want to start from a quickstart template, perform a subgraph migration, or use the contract import feature. 

## Contract Import Tutorial

This walkthrough explains how to initialize an indexer using a single or multiple contracts that are already deployed on a blockchain. This process allows users to quickly and easily start up an indexer with base logic and queryable GraphQL API within less than a minute. Simple supply the contract addresses or ABI files. 

### Intialize your indexer

`cd` into the folder of your choice and run

```bash
envio init
```

Name your indexer

```bash
? Name your indexer:
```

Choose the directory where you would like to setup your project (default is the current directory)

```bash
? Set the directory:  (.) .
```

Select `Contract Import` as the initialization option.

```bash
? Choose an initialization option
  Template
  SubgraphMigration
> ContractImport
[↑↓ to move, enter to select, type to filter]
```

### 1. `Block Explorer` option

```bash
? Would you like to import from a block explorer or a local abi?
> Block Explorer
  Local ABI
[↑↓ to move, enter to select, type to filter]
```

Block Explorer option only requires user to input the address and chain of the contract.
If the contract is verified and deployed on one of the supported chains, this is the quickest setup as it will retrieve all needed contract information from a block explorer.

#### Select the blockchain that the contract is deployed on

```bash
? Which blockchain would you like to import a contract from?
  ethereum-mainnet
  goerli
  optimism
  base
  bsc
> metis
v polygon
[↑↓ to move, enter to select, type to filter]
```

#### Enter in the address of the contract to import

```bash
? What is the address of the contract?
[Use the proxy address if your abi is a proxy implementation]
```

Note if you are using a proxy contract with an implementation, the address should be for the proxy contract.

#### Choose which events to include in the `config.yaml` file

```bash
? Which events would you like to index?
> [x] ClaimRewards(address indexed from, address indexed reward, uint256 amount)
  [x] Deposit(address indexed from, uint256 indexed tokenId, uint256 amount)
  [x] NotifyReward(address indexed from, address indexed reward, uint256 indexed epoch, uint256 amount)
  [x] Withdraw(address indexed from, uint256 indexed tokenId, uint256 amount)
[↑↓ to move, space to select one, → to all, ← to none, type to filter]
```

#### Select the continuation option

```bash
? Would you like to add another contract?
> I'm finished
  Add a new address for same contract on same network
  Add a new network for same contract
  Add a new contract (with a different ABI)
[Current contract: BribeVotingReward, on network: optimism]
```

The `Contract Import` process will prompt the user whether they would like to finish the import process or continue adding more addresses for same contract on same network, addresses for same contract on different network or a different contract.

### 2. `Local ABI` option

```bash
? Would you like to import from a block explorer or a local abi?
  Block Explorer
> Local ABI
[↑↓ to move, enter to select, type to filter]
```

Choosing `Local ABI` option will allow you to point to a JSON file containing the smart contract ABI.
The `Contract Import` process will then populate the required files from the ABI.

> Select this option if the proxy contract has not been verified, which will cause the fetch request from Etherscan client to fail.

#### Specify the directory of JSON file containing ABI

```bash
? What is the path to your json abi file?
```

#### Choose which events to include in the `config.yaml` file

```bash
? Which events would you like to index?
> [x] ClaimRewards(address indexed from, address indexed reward, uint256 amount)
  [x] Deposit(address indexed from, uint256 indexed tokenId, uint256 amount)
  [x] NotifyReward(address indexed from, address indexed reward, uint256 indexed epoch, uint256 amount)
  [x] Withdraw(address indexed from, uint256 indexed tokenId, uint256 amount)
[↑↓ to move, space to select one, → to all, ← to none, type to filter]
```

#### Specify which chain the contract is deployed on

```bash
? Choose network:
> <Enter Network Id>
  ethereum-mainnet
  goerli
  optimism
  base
  bsc
> metis
v polygon
[↑↓ to move, enter to select, type to filter]
```

#### Enter in the name for the contract

```bash
? What is the name of this contract?
```

#### Enter in the address of the contract

```bash
? What is the address of the contract?
[Use the proxy address if your abi is a proxy implementation]
```

Note if you are using a proxy contract with an implementation, the address should be for the proxy.

#### Select the continuation option

```bash
? Would you like to add another contract?
> I'm finished
  Add a new address for same contract on same network
  Add a new network for same contract
  Add a new contract (with a different ABI)
[Current contract: BribeVotingReward, on network: metis]
```

The `Contract Import` process will prompt the user whether they would like to finish the import process or continue adding more addresses for same contract on same network, addresses for same contract on different network or a different contract.

For more information on contract import feature, visit the documentation [here](https://docs.envio.dev/docs/contract-import).


:::info Envio Indexer Examples
Click [here](https://docs.envio.dev/docs/example-uniswap-v3) for Envio Indexer Examples.
:::


## Getting Help

Indexing can be a rollercoaster, especially for more complex use cases. Our engineers are available to help you with your data availability needs.

Join our growing community of elite builders, and find peace of mind with Envio. 

* [Discord](https://discord.gg/mZHNWgNCAc)
* Email: [hello@envio.dev](mailto:hello@envio.dev)

