# The Graph

The Graph is an open-source code developed to streamline the data storage and retrieval within the blockchain network. The Graph helps developers and blockchain organizations increase efficiency and speed on the subject of decentralized applications (Dapps).

Querying of data is performed and maintained using GraphQL, which is one of the best features we have now for designing APIs and providing customers with unique capabilities.

We need to analyze and collect blockchain data before storage so that it can be easily explored and used in the future.

### What is a Subgraph? <a href="#_z1mf3ffm26t0" id="_z1mf3ffm26t0"></a>

A subgraph determines which data is required to be indexed from a blockchain network. Also, a subgraph defines the method of the storage process using lines of code. Simply put, a subgraph is a small part of a bigger network that can act as a list of the desired information retrieved from the Ethereum network.

### Why Do We Need the Graph and Subgraph? <a href="#_beirav397hxg" id="_beirav397hxg"></a>

If you are a developer and you are building applications for Web 3.0, the Graph methodology will essentially help you throughout your work. You can use subgraphs for indexing and querying data from the Ethereum blockchain.

This feature gives developers a hand to present data in their applications and projects and provides efficiency and performance when it comes to developing Dapps.

### How it Works <a href="#_dmur7zkdni9b" id="_dmur7zkdni9b"></a>

As a developer, you can deploy a subgraph that consists of three main parts.

* Manifest: A YAML file (subgraph.YAML) contains the subgraph manifest. This file describes which activities should be performed. It includes useful information about data sources, templates, repositories, and smart contract addresses.
* Schema: A GraphQL (schema.graphql) schema that shows what type of data is going to be stored in a subgraph and how we are going to query it using GraphQL.
* Assembly Script mappings: A subset of Typescript (mapping.ts) that works to convert data coming from the blockchain network to entities defined in our schema.graphql file.

Metis provides users and developers with the Graph’s hosted services. Dapp owners can now deploy subgraphs to the Metis graph-node, and developers can build and publish their open APIs (subgraphs) in a straightforward way. Indexing blockchain data is complex, but you can use the Metis graph-node to simplify the process.

### How to Get Started with the Graph? <a href="#_fl9e765nais6" id="_fl9e765nais6"></a>

In order to start using [the Graph](https://thegraph.com/docs/en/developer/define-subgraph-hosted/) and create a subgraph, we need to install the Graph CLI tool. It can be easily installed using Yarn or NPM package managers.

First, make sure you have the yarn package manager on your Debian-based system. If not, you can install both yarn and npm package managers using the following commands.

```
#nodejs
$ sudo apt-get update
$ sudo apt-get install curl
$ curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
$ sudo apt-get install nodejs

#yarn
$ sudo apt-get update
$ npm install --global yarn
```

Now you can add the Graph CLI by using either the yarn or npm package manager. For more details, please refer to the [official Github repository](https://github.com/graphprotocol/graph-cli) for Graph CLI.

```
$ yarn global add @graphprotocol/graph-cli
$ npm install -g @graphprotocol/graph-cli
```

![](<../.gitbook/assets/0 (2) (1) (1)>)

Please feel free to reach out to our [Help Center](https://metisdao.atlassian.net/servicedesk/customer/portals) if you have any technical questions.
