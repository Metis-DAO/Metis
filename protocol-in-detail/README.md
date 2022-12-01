# Protocol in Detail

### An Introduction to Protocol in Detail <a href="#_th5v4y4qr6h2" id="_th5v4y4qr6h2"></a>

Metis is an Ethereum layer 2 solution that offers several products for developers, DAOs, and people. The products include a wide range of Ethereum blockchain solutions from NFT bridge and the testnet to the Graph and Metis Node all in one platform.

The Metis protocol enables dapp developers and Web 3.0 enthusiasts to get full insight into developing on the Ethereum layer 2 and using cutting-edge blockchain technologies.

Simply speaking, Metis is a Layer 2 scaling protocol for Ethereum applications and dapp developers. It focuses on providing faster and cheaper transactions that result in faster development of the Web 3.0 ecosystem and solutions. Anyone from Web 3.0 supporters to senior developers can access the Metis Layer 2 solution and integrate it into their projects.

In this part of the Metis docs, we aim at exploring the Metis protocol and explaining how it works under the hood.

### Take the First Ideas <a href="#_mx7klhx2mcxh" id="_mx7klhx2mcxh"></a>

Metis is an infrastructure that means it looks, feels, and behaves like the Ethereum blockchain. But, it’s cheaper, faster, easier to use, and accessible to anyone. Smart contract deployment or using Ethereum nodes are complex, but you can take Metis solutions into account to help yourself and get seamless transitions.

Solidity smart contracts can run on the Metis Layer 2 blockchain as they run on Layer 1. Similarly, off-chain apps can interact with the Metis platform with a little configuration and an updated RPC endpoint.

### System Overview <a href="#_4sbt6cffc3qg" id="_4sbt6cffc3qg"></a>

Smart contracts in the Metis protocol are divided into some specific components. Each component acts as an important part of the whole platform and serves to make the Layer 2 blockchain become true.

Here are the key elements of the Metis protocol which we will discuss in more detail in the next sections.

#### Layer 1 <a href="#_1zxpqlrpylpe" id="_1zxpqlrpylpe"></a>

* Messaging: Contracts that facilitate a message passing from L1 to L2.
* Rollup: Contracts on the L1 that hold the ordering of L2 transactions and commitments to the associated L2 state roots. These are contracts that run on the Ethereum mainnet.
* Verification: A stub of the “real” Bond Manager that allows the OVM proposer to submit state root batches.

#### Layer 2 <a href="#_5b98v8hvl7bv" id="_5b98v8hvl7bv"></a>

* Messaging: Contracts that facilitate message passing from L2 to L1.
* Predeploys: These contracts are similar to Ethereum precompiles and they are written in Solidity. These contracts can be found at addresses with 0x42 prefixes.

#### MVM <a href="#_esuw4mlrsawm" id="_esuw4mlrsawm"></a>

* Verifier: These are a proven set of contracts that implement the Metis Virtual Machine.

### Layer 1 <a href="#_wfj6c9efgtlo" id="_wfj6c9efgtlo"></a>

L1 itself is composed of different sections that facilitate messaging and ordering of contracts passing from L1 to L2. Layer 1 holds contracts that are categorized under these three important parts:

* L1 messaging
* L1 rollup
* Verification

### Layer 1 Messaging <a href="#_j9xtzkpeu2px" id="_j9xtzkpeu2px"></a>

There is a need for L1 messaging to easily and securely make connections between L1 and L2. This is an advantage of the Metis protocol that the contracts on the L1 messaging have passed through several audits and provide safe and secure connections from L1 to L2.

You need to be familiar with the following contracts that work in the Layer 1 messaging.

#### L1 Cross Domain Messenger <a href="#_mnafjz4rdab2" id="_mnafjz4rdab2"></a>

The role of the L1 Cross Domain Messenger contract is to send messages from L1 to L2. It also relays messages from L2 to L1 and is responsible for maintaining rejected messages that are passing from L1 to L2. If a contract sent from L1 to L2 is rejected because of exceeding the L2 epoch gas limit, it can be resubmitted via the “replay” function of this contract.

#### L1 Standard Bridge <a href="#_6bp7hyu85s3x" id="_6bp7hyu85s3x"></a>

The L1 Standard Bridge (ETH and ERC20) contract stores deposited L1 funds and standard tokens that users are willing to bridge from L1 to L2 and are in use on the L2. The contract synchronizes with a corresponding L2 bridge to get information about deposits and newly finalized withdrawals.

### L1 Rollup Contracts <a href="#_8qnpd64duq8g" id="_8qnpd64duq8g"></a>

L1 Rollup is composed of a set of contracts that runs on the L1 Ethereum mainnet. This set of contracts have an important role as they store ordered lists of transactions and commitments:

* An ordered list of all transactions that are applied to the L2 state.
* The proposed state root that results from the application of each transaction.
* Transactions sent from L1 to L2 that are pending in the ordered list.

In the case of L1 Rollup, the chain includes the following concrete contracts.

### Canonical Transaction Chain CTC <a href="#_lnadqbzb0jzy" id="_lnadqbzb0jzy"></a>

The CTC defines the ordering of transactions by writing them to ‘CTC:batches’, an instance of the Chain Storage Container (CSC). Note that the CTC is an append-only log of transactions that must be applied to the Rollup state. The CTC allows any account or user to ‘enqueue’ L2 transactions that the sequencer will eventually append to the rollup state.

### State Commitment Chain SCC <a href="#_1ua63thuvjwn" id="_1ua63thuvjwn"></a>

The SCC plays an important role in the L1 as it contains a list of proposed state roots. The proposers assert to be a result of each transaction coming from the CTC.

Elements here and transactions in the CTC have 1:1 correspondence. Elements should be unique state roots calculated off-chain by applying canonical transactions one by one.

### Chain Storage Container CSC <a href="#_yojyujjal1ck" id="_yojyujjal1ck"></a>

The CSC gives reusable storage and provides its owner contract with read, write, and delete functionality. The storage is in the form of a “Ring Buffer” data structure that will overwrite storage slots that are no longer needed.

This will provide gas efficiency gains and better functionality when it comes to performing transactions on L1.

There are 2 different CSC types that will be deployed on L1, two of them are controlled by the CTC and one is controlled by the SCC:

* CTC: Stores transaction batches
* SCC: Stores chain state batches

### Layer 1 Verification <a href="#_4vbtg0nj0qvz" id="_4vbtg0nj0qvz"></a>

The L1 Verification proves security and trust for the Chain. The Chain consists of a list of proposed state roots that comes from each transaction. So, the L1 Verification helps us prove the honesty of these proposals.

* Transaction approved: If executing a transaction results in a correct proposed state root, the verifier will receive a reward from funds that eventually a sequencer can take it as a bond.
* Transaction not approved: If a proposed state root is not verified, then the verifier initiates a transaction result challenge.

### Bond Manager <a href="#_shy7rtm9ioh8" id="_shy7rtm9ioh8"></a>

The Bond Manager contract manages deposits in the form of an ERC20 token. Funds and gas fees from bonded proposers are handled by the Bond Manager and a stub of the “real” Bond Manager works in this part for now. It does nothing but allows the “OVM\_Proposer” to submit state root batches safely and trustfully.

### Layer 2 <a href="#_bpw4y27x91a9" id="_bpw4y27x91a9"></a>

There is a set of important contracts on L2, including the messaging and the predeploys. Layer 2 was built on the L1 blockchain using smart contracts and it helps improve the network scalability and transaction speed.

Layer 2 consists of the following parts:

* L2 messaging
* L2 predeploys

### Layer 2 Messaging <a href="#_fvjpe3xlfn8t" id="_fvjpe3xlfn8t"></a>

L2 messaging is responsible for handling and facilitating messages passing from L2 to L1. So, it is crucial to make high-speed and safe connections between L2 and L1.

L2 messaging consists of the following parts.

#### Layer 2 Cross Domain Messenger <a href="#_pj3ot0f8imz5" id="_pj3ot0f8imz5"></a>

The L2 CDM is a contract that sends messages from L2 to L1. It can be called the entry point for L2 messages coming from the L1 Cross Domain Messenger.

#### L2 Standard Bridge <a href="#_rxujoz2tcay7" id="_rxujoz2tcay7"></a>

The L2 Standard Bridge works together with the L1 Standard Bridge to enable ETH and ERC20 transitions between L1 and L2. It’s a contract that acts as a miner for new tokens when it hears deposits into the L1 Standard Bridge. The L2 Standard Bridge also acts as the burner of tokens intended for withdrawals, informing the L1 bridge to release L1 funds.

#### L2 Standard Token Factory <a href="#_hg1hjhjv41mp" id="_hg1hjhjv41mp"></a>

The L2 Standard Token Factory is a factory contract that creates standard L2 token representations of L1 to work on the standard bridge.

### Layer 2 Predeploys <a href="#_f0cymyrlt9rk" id="_f0cymyrlt9rk"></a>

L2 Predeploys are a set of contracts written in Solidity language and are something like Ethereum precompile contracts. L2 Predeploys handle specific common operations as stated below.

#### OVM\_DeployerWhitelist <a href="#_f32zkox22y36" id="_f32zkox22y36"></a>

The Deployer Whitelist does a temporary operation to provide additional safety during the initial phases of our mainnet roll out. It is owned by the Metis team and holds accounts that are allowed to deploy contracts on the Metis L2.

If a deployer’s address is whitelisted, then the execution manager allows an ovmCREATE or ovmCREATE2 operation to proceed.

#### OVM\_ETH <a href="#_9p1z4jwqq7uh" id="_9p1z4jwqq7uh"></a>

The OVM ETH predeploy provides an ERC20 interface for ETH deposited to L2. Unlike L1, L2 accounts do not have a balance field.

#### OVM\_GasPriceOracle <a href="#_ec7f5764w7sq" id="_ec7f5764w7sq"></a>

This contract determines the current L2 gas price. The amount of the gas fee depends on how the network is congested at the moment. In this measure, the sequencer works to know how much fees are required for transactions. Note that when the system is congested, the L2 gas prices will increase accordingly.

All public variables are set while generating the initial L2 state. The constructor doesn't run in practice as the L2 state generation script uses the deployed bytecode instead of running the initcode.

#### OVM\_L2ToL1MessagePasser <a href="#_wkchzayupy38" id="_wkchzayupy38"></a>

The L2 to L1 Message Passer is a utility contract that facilitates an L1 proof of a message on L2. The L1 Cross Domain Messenger performs this proof in its \_verifyStorageProof function. As a result, it verifies the existence of the transaction hash in the ‘sendMessages’ mapping of the L2ToL1MessagePasser contract.

#### OVM\_SequencerFeeVault <a href="#_289dihyxkusv" id="_289dihyxkusv"></a>

The Sequencer Fee Vault is a simple contract that is used for fees paid to the sequencer. The fees are likely to be replaced in the future, but “good enough for now”.

### Metis Virtual Machine MVM <a href="#_j7v3c1xsx04i" id="_j7v3c1xsx04i"></a>

Here is a set of contracts that implement the Metis Virtual Machine. Metis uses optimistic roll up with enhanced L2 roll up. The wait time is significantly resolved by the Metis protocol because of using the Metis Virtual Machine. This virtual machine is compatible with the EVM.

### MVM\_Verifier <a href="#_5sifi1ychejv" id="_5sifi1ychejv"></a>

The Fraud Verifier contract coordinates the entire fraud proof verification process. If the fraud proof is successful, it prunes any state batches from the State Commitment Chain which were published after the fraudulent state root.
