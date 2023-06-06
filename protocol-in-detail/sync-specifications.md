# Synchronization Specifications

### Synchronization Specifications <a href="#_550ndvpfbbxt" id="_550ndvpfbbxt"></a>

Transaction data can be indexed from either L1 or L2. This data is used for doing the execution process which generates the state. Synchronization is the process of scanning the network for new transactions and changes in the blockchain that helps us guarantee the information of transactions and their authenticity.

* The benefit of synching transactions from L2 is that we can sync the data before the transactions are batch submitted.
* The benefit of synching from L2 is that the transactions are all final which will provide a fraud-proof state.

### Retrieving Relevant Data <a href="#_oirydeb8bdx7" id="_oirydeb8bdx7"></a>

We need to retrieve the following information reliably in order to perform a reliable synchronization:

* All transactions enqueued for inclusion in the CanonicalTransactionChain that are in the order in which they were enqueued.
* All transactions included in the CanonicalTransactionChain that are in the order in which they were included.
* All state roots included in the StateCommitmentChain that are in the order in which they were included.

All relevant data can be retrieved by parsing the data from the following functions:

* `CanonicalTransactionChain.enqueue`
* `CanonicalTransactionChain.appendQueueBatch`
* `CanonicalTransactionChain.appendSequencerBatch`
* `StateCommitmentChain.appendStateBatch`

### Enqueued Transactions <a href="#_n8rnqzedtv9m" id="_n8rnqzedtv9m"></a>

Note that transactions are “enqueued” when users make calls to the `CanonicalTransactionChain.enqueue` function.

All function calls to this function can be detected by searching for [TransactionEnqueued](https://github.com/MetisProtocol/mvm/blob/develop/specs/l2geth/transaction-indexer.md#transactionenqueued) events. All relevant data for transactions can be pulled out of that specific event, and here is a pseudocode function for doing this job:

```
function parseTransactionEnqueuedEvent(
event: TransactionEnqueued,
): EnqueuedTransaction {
return {
queueIndex: event.queueIndex,
timestamp: event.timestamp,
blockNumber: getBlockNumber(event),
l1QueueOrigin: QueueOrigin.L1TOL2_QUEUE,
l1TxOrigin: event.l1TxOrigin
entrypoint: event.target,
gasLimit: event.gasLimit,
data: event.data
}
}
```

Altogether, the process of parsing and indexing the data is pretty straightforward:

1. Listen to [TransactionEnqueued](https://github.com/MetisProtocol/mvm/blob/develop/specs/l2geth/transaction-indexer.md#transactionenqueued) events.
2. Parse each found event into EnqueuedTransaction structures.
3. Store each event based on its queueIndex field.

### Transactions Via appendQueueBatch <a href="#_7qgwxv6nwe5" id="_7qgwxv6nwe5"></a>

:::Note that the `appendQueueBatch` is currently disabled on the mainnet.:::

Transactions can be inserted into the Canonical Transaction Chain using either the `appendQueueBatch` or `appendSequencerBatch` functions.

`appendQueueBatch` is used to move enqueued transactions to a canonical set of transactions. Until the process is done, these enqueued transactions are not considered as a part of the L2 history.

Once the `appendQueueBatch` function has been called, a `TransactionBatchAppended` event is emitted. This is followed by a `QueueBatchAppended` event in the same transaction. (The index of the second event is equal to the index of the first event plus one.)

These types of transactions do not include the complete forms of various transactions that were appended by the function call. As a result, we need to pull the required information from a combination of events. Also, we can count on `EnqueuedTransaction` objects that have been previously parsed from the `enqueue` process.

Here are some lines of pseudocode for parsing this list of transactions that provide us with the required information:

```
function parseQueueBatchAppendedEvent(
event: QueueBatchAppended
): Transaction[] {
// Get the `TransactionBatchAppended` event. Really should be turned into a
// single event to avoid having to do this extra network request.
const event2: TransactionBatchAppended = getEventByIndex(
getEventIndex(event) - 1
)
const transactions: Transaction[] = []
for (let i = 0; i < event.numQueueElements) {
// Note that this places an assumption on how events are parsed. This
// only works if enqueued transactions are parsed before
// `appendQueueBatch` events.
const enqueuedTransaction: EnqueuedTransaction = getEnqueuedTransactionByIndex(
event.startingQueueIndex + i
)
transactions.push({
l1QueueOrigin: QueueOrigin.L1TOL2_QUEUE,
timestamp: enqueuedTransaction.timestamp,
blockNumber: enqueuedTransaction.blockNumber,
l1TxOrigin: enqueuedTransaction.l1TxOrigin,
entrypoint: enqueuedTransaction.entrypoint,
gasLimit: enqueuedTransaction.gasLimit,
data: enqueuedTransaction.data
})
}
// TODO: Add parsing batches.
return transactions
}
```

### Transactions Via appendSequencerBatch <a href="#_frr6x6si7ood" id="_frr6x6si7ood"></a>

Using `appendSequencerBatch` is another method in which transactions can be inserted into the Canonical Transaction Chain. The main benefit of `appendSequencerBatch` is that it uses a custom encoding scheme in order to enhance efficiency and it does not have any explicit parameters.

The function parses `calldata` internally, directly using the following format:

* Bytes 0-3 (4 bytes) of `calldata` are the 4-byte function selector derived from keccak256 (“appendSequencerBatch()”). Just skip these four bytes.
* Bytes 4-8 (5 bytes; uint40) describe the index of the Canonical Transaction Chain that the batch of transactions expects to follow.
* Bytes 9-11 (3 bytes; uint24) are the total number of elements that the sequencer wants to append to the chain.
* Bytes 12-14 (3 bytes; uint24) are the total number of “batch contexts” that are effective timestamp/block numbers to be assigned to a given set of transactions.

After byte 14, we have a series of encoded “batch contexts”. Each batch context is exactly 16 bytes. Note that the number of contexts comes from the bytes 12-14.

Each context follows the structure stated below:

* Bytes 0-2 (3 bytes; uint24) are the number of sequencer transactions that will use this batch context.
* Bytes 3-5 (3 bytes; uint24) are the number of queue transactions that will be inserted into the chain after the above-mentioned sequencer transactions.
* Bytes 6-10 (5 bytes; uint40) are a timestamp that will be assigned to the above-mentioned sequencer transactions.
* Bytes 11-15 (5 bytes; uint40) are the block number that will be assigned to the above-mentioned sequencer transactions.

After the batch context section, we have a series of dynamically sized transactions. Each transaction consists of the following information:

* Bytes 0-2 (3 bytes; uint24) are the total size of the coming transaction data.
* Some arbitrary data of a length equal to that, described by the first 3 bytes.

We can represent the input as an object roughly equivalent to the following json-ish piece of code:

```
interface AppendSequencerBatchParams {
sighash: 4 bytes,
shouldStartAtElement: 5 bytes,
totalElementsToAppend: 3 bytes,
numContexts: 3 bytes,
contexts: Array<{
numSequencedTransactions: 3 bytes,
numSubsequentQueueTransactions: 3 bytes,
ctxTimestamp: 5 bytes,
ctxBlockNumber: 5 bytes
}>,
transactions: Array<{
txDataLength: 3 bytes,
txData: txDataLength bytes
}>
}
```

The decoding function in the pseudocode format is as follows:

```
function decode(calldata: bytes): AppendSequencerBatchParams {
const sighash = calldata[0:4]
const shouldStartAtElement = uint40(calldata[4:9])
const totalElementsToAppend = uint24(calldata[9:12])
const numContexts = uint24(calldata[12:15])
let ptr = 15
const contexts = []
for (let i = 0; i < numContexts; i++) {
contexts.push({
numSequencedTransactions: uint24(calldata[ptr:ptr+3])
numSubsequentQueueTransactions: uint24(calldata[ptr+3:ptr+6]),
ctxTimestamp: uint40(calldata[ptr+6:ptr+11]),
ctxBlockNumber: uint40(calldata[ptr+11:ptr+16])
})
ptr = ptr + 16
}
const transactions = []
while (ptr < length(calldata)) {
const txDataLength = uint24(calldata[ptr:ptr+3])
transactions.push({
txDataLength: txDataLength,
txData: calldata[ptr+3:ptr+3+txDataLength]
})
ptr = ptr + 3 + txDataLength
}
return {
sighash: sighash,
shouldStartAtElement: shouldStartAtElement,
totalElementsToAppend: totalElementsToAppend,
numContexts: numContexts,
contexts: contexts,
transactions: transactions,
}
}
```

The encoding function in the pseudocode format is as follows:

```
function encode(params: AppendSequencerBatchParams): bytes {
let calldata = bytes()
calldata[0:4] = bytes4(params.sighash)
calldata[4:9] = bytes5(params.shouldStartAtElement)
calldata[9:12] = bytes3(params.totalElementsToAppend)
calldata[12:15] = bytes3(params.numContexts)
let ptr = 15
for (const context of params.contexts) {
calldata[ptr:ptr+3] = bytes3(context.numSequencedTransactions)
calldata[ptr+3:ptr+6] = bytes3(context.numSubsequentQueueTransactions)
calldata[ptr+6:ptr+11] = bytes5(context.ctxTimestamp)
calldata[ptr+11:ptr+16] = bytes5(context.ctxBlockNumber)
ptr = ptr + 16
}
for (const transaction of params.transactions) {
const txDataLength = transaction.txDataLength
calldata[ptr:ptr+3] = bytes3(txDataLength)
calldata[ptr+3:ptr+3+txDataLength] = bytes(transaction.data)
ptr = ptr + 3 + txDataLength
}
return calldata
}
```

### The Method <a href="#_gmny8kmidgqm" id="_gmny8kmidgqm"></a>

When the sequencer calls `appendQueueBatch`, contexts are processed one by one.

* We first append `numSequencedTransactions` for each of these contexts which are popped off the transactions array.

Note that each of these transactions has the following format:

```
Transaction({
timestamp: context.ctxTimestamp,
blockNumber: context.ctxBlockNumber,
l1QueueOrigin: QueueOrigin.SEQUENCER_QUEUE,
l1TxOrigin: 0x0000000000000000000000000000000000000000,
entrypoint: 0x4200000000000000000000000000000000000005,
gasLimit: OVM_ExecutionManager.getMaxTransactionGasLimit(),
data: tx.txData,
})
```

* Next, we pull `numSubsequentQueueTransactions` in from the queue by referencing the queue. The process should be repeated for every provided context.

Note the following points when working with this process:

* `appendSequencerBatch` can be detected and parsed by looking for [SequencerBatchAppended](https://github.com/MetisProtocol/mvm/blob/develop/specs/l2geth/transaction-indexer.md#sequencerbatchappended) events. Each of these events will always be immediately preceded by a [TansactionBatchAppended](https://github.com/MetisProtocol/mvm/blob/develop/specs/l2geth/transaction-indexer.md#transactionbatchappended) event.
* Somewhat like with `appendQueueBatch`, we have to be careful in pulling all the relevant information out from these two events and from the previously parsed `EnqueuedTransactions`.
* We need access to the `calldata` sent to `appendSequencerBatch`. We can retrieve the data by making a call to the [debug\_traceTransaction](https://geth.ethereum.org/docs/rpc/ns-debug#debug\_tracetransaction) or an equivalent endpoint that exposes call traces. In the case that a client does not have access to the `debug_traceTransaction` endpoint, the `calldata` can only be retrieved if `appendSequencerBatch` is called directly. This call is by an externally owned account.
* When a client detects a `SequencerBatchAppended` event, they should pull the preceding `TransactionBatchAppended` event. Then they should retrieve the `calldata` and decode it using the above-mentioned decoding scheme.
* The client should validate the input under the assumption that data from the L1 node is not reliable.

The code for parsing the event in the pseudocode format is as follows:

```
function parseSequencerBatchAppendedEvent(
event: QueueBatchAppended
): Transaction[] {
// Get the `TransactionBatchAppended` event. Really should be turned into a
// single event to avoid having to do this extra network request.
const event2: TransactionBatchAppended = getEventByIndex(
getEventIndex(event) - 1
);
const calldata: bytes = getCalldataByTransaction(getTransaction(event));
const params: AppendSequencerBatchParams = decode(calldata);
let sequencerTransactionCount = 0;
let queueTransactionCount = 0;
const transactions: Transaction[] = [];
for (const context of params.contexts) {
for (let i = 0; i < context.numSequencerTransactions; i++) {
transactions.push({
l1QueueOrigin: QueueOrigin.SEQUENCER_QUEUE,
timestamp: context.ctxTimestamp,
blockNumber: context.ctxBlockNumber,
l1TxOrigin: 0x0000000000000000000000000000000000000000,
entrypoint: 0x4200000000000000000000000000000000000005,
gasLimit: OVM_ExecutionManager.getMaxTransactionGasLimit(),
data: params.transactions[sequencerTransactionCount],
});
sequencerTransactionCount = sequencerTransactionCount + 1;
}
for (let i = 0; i < context.numSubsequentQueueTransactions; i++) {
// Note that this places an assumption on how events are parsed. This
// only works if enqueued transactions are parsed before
// `appendQueueBatch` events.
const enqueuedTransaction: EnqueuedTransaction =
getEnqueuedTransactionByIndex(
event.startingQueueIndex + queueTransactionCount
);
transactions.push({
l1QueueOrigin: QueueOrigin.L1TOL2_QUEUE,
timestamp: enqueuedTransaction.timestamp,
blockNumber: enqueuedTransaction.blockNumber,
l1TxOrigin: enqueuedTransaction.l1TxOrigin,
entrypoint: enqueuedTransaction.entrypoint,
gasLimit: enqueuedTransaction.gasLimit,
data: enqueuedTransaction.data,
});
queueTransactionCount = queueTransactionCount + 1;
}
}
// TODO: Add parsing batches.
return transactions;
}
```

Please feel free to reach out to our [Help Center](https://metisdao.atlassian.net/servicedesk/customer/portals) if you have any technical questions.
