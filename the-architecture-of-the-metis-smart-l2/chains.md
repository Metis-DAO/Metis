# Chains

### Chains <a href="#_rb29vhiy5d83" id="_rb29vhiy5d83"></a>

We need to consider different chains within the Metis architecture. Each chain has a specific role in the blockchain network and performs one-by-one actions to achieve security, transaction speed, and a reliable decentralized network.

We are going to describe the Transaction Queue, the State Commitment Chain, and the Canonical Transaction Chain.

### The Chains List <a href="#_78uk2qa0zfc1" id="_78uk2qa0zfc1"></a>

Chains

* Transaction Queue
  * The structure of the Transaction Queue
    * Transaction Queue processes
    * Appending to the Transaction Queue
* Canonical Transaction Chain
  * Canonical Transaction Chain structure
  * Verifying transaction inclusion in the CTC
  * Updating the CTC
    * Appending to the CTC by appendSequencerBatch()
    * Transaction ordering requirements
      * Force inclusion period rules
      * Context properties
  * Appending to the CTC by appendQueueBatch()
    * appendQueueBatch() requirements
* State Commitment Chain
  * SCC structure
  * Verifying state root inclusion to the SCC
    * appendStateBatch requirements
  * Deleting batches from the SCC
    * deleteStateBatch() requirements

### Transaction Queue <a href="#_a62qrr8u8nsh" id="_a62qrr8u8nsh"></a>

The Transaction Queue is an append-only array of transaction data that must eventually be included in the Canonical Transaction Chain. Note that the Transaction Queue itself does not describe the L2 state, but it serves as a source of transaction inputs to the CTC.

### The Structure of the Transaction Queue <a href="#_vwr3xduvluay" id="_vwr3xduvluay"></a>

2 bytes32 entries are stored for each Transaction Queue.

* 1: The first entry is `transactionHash` defined in the Solidity language as:

```
bytes32 transactionHash = keccak256(
abi.encode(
msg.sender,
_target,
_gasLimit,
_data
)
);
```

* 2: The second entry is timestampAndBlockNumber which records the TIMESTAMP and NUMBER values in the EVM at write time. This entry is defined in the Solidity language as:

```
bytes32 timestampAndBlockNumber;
assembly {
timestampAndBlockNumber := timestamp()
timestampAndBlockNumber := or(timestampAndBlockNumber, shl(40, number()))
}
```

There is a [QueueElement](https://github.com/MetisProtocol/mvm/blob/develop/specs/protocol/data-structures.md#queueelement) type which is also used to represent this data.

#### Transaction Queue Processes <a href="#_v5mqdbky9qy5" id="_v5mqdbky9qy5"></a>

The Transaction Queue is append-only and the only allowed update operation is appending to it. The Queue has 2 distinct purposes:

1. Resistance against a censorious Sequencer (See Actors and Roles)
2. Sending messages (i.e. deposits) from L1 to L2 (See Cross Domain Messaging)

#### Appending to the Transaction Queue <a href="#_50bi7td5l969" id="_50bi7td5l969"></a>

Only Proxy\_\_OVM\_L1CrossDomainMessenger address can append to the Transaction Queue by calling the CTC’s enqueue() function. Any account who want to enqueue may send a cross domain message by calling a L1 contract which checked by MVM\_DiscountOracle.isXDomainSenderAllowed.

```
function enqueue(
address _target,
uint256 _gasLimit,
bytes _data
)
```

Where the parameters are:

* `address _target`: The target L2 contract to send the transaction to.
* `Uint256 _gasLimit`: The gas limit for the enqueued L2 transaction.
* `bytes _data`: Arbitrary calldata for the enqueued L2 transaction.

### Canonical Transaction Chain <a href="#_subnpt4i4hcw" id="_subnpt4i4hcw"></a>

Canonical Transaction Chain is an essential part of verifying the L2 state. It is an array of transactions that must be processed in-order to determine and verify the L2 state. The manager can add, delete and update the transactions data, update queue data when a fraud proof accepted in challege. Here, the transaction data is compressed into batches to reduce storage costs.

Note that there are no blocks here and the CTC is just an ordered list of transactions.

### Canonical Transaction Chain Structure <a href="#_7hmpj34ehjan" id="_7hmpj34ehjan"></a>

Entries in the CTC are in type `bytes32 batchHeaderHash`, defined as the hash of the elements of a [ChainBatchHeader](https://github.com/MetisProtocol/mvm/blob/develop/specs/protocol/data-structures.md#chainbatchheader). Note that the `batchIndex` is not included.

```
keccak256(
abi.encode(
_batchHeader.batchRoot,
_batchHeader.batchSize,
_batchHeader.prevTotalElements,
_batchHeader.extraData
)
);
```

Additionally, the CTC maintains its ‘global context’ in a bytes27 `latestBatchContext`, which encodes the fields of the Extra Data structures as follows:

```
bytes27 extraData;
assembly {
extraData := _totalElements
extraData := or(extraData, shl(40, _nextQueueIndex))
extraData := or(extraData, shl(80, _timestamp))
extraData := or(extraData, shl(120, _blockNumber))
extraData := shl(40, extraData)
}
```

### Verifying Transaction Inclusion in the CTC <a href="#_a3dtuer8og37" id="_a3dtuer8og37"></a>

Verifying transactions are processed by the `verifyTransaction` function that returns a boolean. This process indicates if a transaction is included in the chain or not:

```
function verifyTransaction(
Transaction _transaction,
TransactionChainElement _txChainElement,
ChainBatchHeader _batchHeader,
ChainInclusionProof _inclusionProof
)
returns(
bool
)
```

The parameters are as follows:

* `Transaction _transaction`: The transaction to be verified.
* `TransactionChainElement _txChainElement`: The transaction chain element corresponding to the transaction.
* `ChainBatchHeader _batchHeader`: The header of the batch that the transaction was included in.
* `ChainInclusionProof _inclusionProof`: The inclusion proof for the provided transaction chain element.

### Updating the CTC <a href="#_bulhr316cad" id="_bulhr316cad"></a>

The Transaction Queue is append-only and the only allowed update operation is appending to it. This process can be done using 2 methods as follows:

* `appendSequencerBatch()`
* `appendQueueBatch()`

#### Appending to the CTC by appendSequencerBatch() <a href="#_1luivkl5xj23" id="_1luivkl5xj23"></a>

The Sequencer appends transactions to the chain in batches by calling the CTC's `appendSequencerBatch()` function, defined in the Solidity language as:

function `appendSequencerBatch()`

The data provided MUST conform to a custom encoding scheme (which is used for efficiency reasons).&#x20;

The BatchContext data provided by the Sequencer will be used to determine the ordering of transactions in the CTC:

* First `BatchContext.numSequencedTransactions` are added from the `_transactionDataFields`.
* Then `BatchContext.numSubsequentQueueTransactions` are added from the queue.

This process is repeated until[ totalElements](https://github.com/MetisProtocol/mvm/blob/develop/specs/protocol/data-structures.md#extra-data) transactions have been appended.

#### Transaction ordering requirements <a href="#_q5p97km0pan5" id="_q5p97km0pan5"></a>

The following constraints MUST be imposed on the ordering of the Sequencer and Queue transactions (for the purpose of preventing a malicious Sequencer from attempting to censor transaction):

**Force Inclusion Period rules**

The Force Inclusion Period is a storage variable defined in the CTC. It is expected to be on the order of 10 to 60 minutes.

1. If any queue elements are older than the Force Inclusion Period, they must be appended to the chain before any Sequencer transaction is processed.
2. The Sequencer MUST not be able to insert Sequencer Transactions older than Force Inclusion Period.

**Context properties**

`BatchContext.blockNumber` and `BatchContext.timestamp` MUST be:

* monotonically increasing
* less than or equal to the L1 blockNumber and timestamp when the `appendSequencerBatch()` function is called
* less than or equal to the blockNumber and timestamp on all QueueElements

An important high-level property that emerges from these rules is that Queue transactions will always have the same timestamp/blocknumber on L2 as the L1 block during the time they were enqueued.

_Note: There may be some implicit requirements missing from here_.

### Appending to the CTC by appendQueueBatch() <a href="#_dwb95lxftvu9" id="_dwb95lxftvu9"></a>

Any account MAY append transactions from the Queue to the CTC by calling the `appendQueueBatch()` function:

```
function appendQueueBatch(
uint256 _numQueuedTransactions
)
```

Where the parameter is:

* `uint256 _numQueuedTransactions`: The number of transactions from the queue to be appended to the CTC.

#### appendQueueBatch() Requirements <a href="#_sca35r7pr6sn" id="_sca35r7pr6sn"></a>

Transactions MUST have been added to the Queue earlier than now - forceInclusionPeriod. If a transaction is newer, appendQueueBatch() will revert.

### State Commitment Chain <a href="#_ridp806zks55" id="_ridp806zks55"></a>

The State Commitment Chain (SCC) is an array of StateCommitmentBatches, where each batch is a Merkle root derived from an array of state roots.

In practice, the SCC should be append-only, but in the event of a successful fraud-proof, the state may be rolled back to the most recent honest state.

### SCC Structure <a href="#_hlc60fdc391x" id="_hlc60fdc391x"></a>

The SCC is an array of[ ChainBatchHeaders](https://github.com/MetisProtocol/mvm/blob/develop/specs/protocol/data-structures.md#chainbatchheader).

### Verifying State Root Inclusion in the SCC <a href="#_pek6vzld89i9" id="_pek6vzld89i9"></a>

The SCC's `verifyTransaction` function returns a boolean indicating whether a transaction is included in the chain or not:

```
function verifyStateCommitment(
bytes32 _element,
ChainBatchHeader _batchHeader,
ChainInclusionProof _proof
)
returns (
bool _verified
);
```

Where the parameters are:

* `bytes32 _element`: The hash of the element to verify a proof for.
* `ChainBatchHeader _batchHeader`: The header of the batch in which the element was included.
* `ChainInclusionProof _proof`: Merkle inclusion proof for the element.

### Updating the SCC: Appending Batches to the SCC <a href="#_lhnodixk6ims" id="_lhnodixk6ims"></a>

An account MAY act as a Proposer and call the SCC's `appendStateBatch()` function to commit to L2 state roots (Only if it is approved by the Bond Manager contract).

_At this time, the Bond Manager will only approve the Sequencer. In the future this will be extended to any account which has deposited the required collateral._

```
function appendStateBatch(
bytes32[] _batch,
uint256 _shouldStartAtElement
)
```

The logic of the `appendStateBatch()` function will compute the Merkle root of the batches and append it to the SCC.

#### appendStateBatch() Requirements <a href="#_tkobe3klil0" id="_tkobe3klil0"></a>

* The batch may not be empty, at least one new state root must be added.
* The `BondManager.isCollateralized(msg.sender)` must return true.
* The resulting number of elements in the SCC (i.e. all state commitments, not batches) must be less than or equal to the number of elements (i.e. transactions) in the CTC.

### Updating the SCC: Deleting Batches from the SCC <a href="#_8s1usfcjjthb" id="_8s1usfcjjthb"></a>

In the event of a successful fraud-proof, the Fraud Verifier contract may delete the State Batch which contains the fraudulent state root, by calling the `deleteStateBatch()` function:

```
function deleteStateBatch(
ChainBatchHeader _batchHeader
)
```

Where the parameters are:

* `ChainBatchHeader _batchHeader`: The header of the state batch to delete.

Note that this may result in valid state commitments prior to the fraudulent state being deleted. This is OK, the next honest proposer can resubmit them.

#### deleteStateBatch() Requirements <a href="#_k0dxh2ev6fo3" id="_k0dxh2ev6fo3"></a>

* The caller MUST be the Fraud Verifier contract.
* The fraud-proof window must not have passed at runtime.
* The batch header must be valid.

Please feel free to reach out to our [Help Center](https://metisdao.atlassian.net/servicedesk/customer/portals) if you have any technical questions.
