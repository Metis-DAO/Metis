# Sequencers

### Sequencers <a href="#_72f5dje02ne5" id="_72f5dje02ne5"></a>

A sequencer plays a key role in the blockchain ecosystem. The sequencer is responsible for sorting transactions and it records the transactions on its local blockchain platform.

According to the diagram, an L2 user gives a request by performing a transaction. This is the role of the sequencer to get the transaction and sort it in order to send it to `CanonicalTransactionChain`.

![](https://3353191897-files.gitbook.io/\~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-McQf9YqnjTLsZKxr\_aB%2Fuploads%2F8ASB904ZkXTffaxUosgO%2FSequencer-L2%20Transaction.png?alt=media\&token=5aa18ab7-6e32-4152-830d-b1ad34d1dd64)

* The sequencer receives transactions from blockchain users.
* The transactions are recorded on the blockchain platform of the sequencer.
* It sorts the transactions and periodically sends them to the `CanonicalTransactionChain` contract.
* This way, we ensure the transaction ordering and data availability.

### Unusual Cases <a href="#_vn2oooueag3" id="_vn2oooueag3"></a>

Note that if the sequencer is unavailable to work or is malicious, L2 users can directly post transactions to the `CanonicalTransactionChain` contract.

In order to prevent edge-case conditions where users include a transaction that may invalidate the batch, a time delay is required and considered.

![](https://3353191897-files.gitbook.io/\~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-McQf9YqnjTLsZKxr\_aB%2Fuploads%2FoWjNVPieDwHsQiqahlYa%2FSequencer-Fraud%20Proof%20Part%204%20-%20PostExecution-Malicious%20Sequencer.png?alt=media\&token=21ef7343-1008-4a42-bf87-5805e1ed0efe)

### What Is the Role of the State Commitment? <a href="#_ph8gr5crjdgy" id="_ph8gr5crjdgy"></a>

![](https://3353191897-files.gitbook.io/\~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-McQf9YqnjTLsZKxr\_aB%2Fuploads%2FNvIY1WT9D2jhBXp2IWdo%2FSequencer-Fraud%20Proof%20Part%204%20-%20PostExecution-L2%20Transaction-State%20Commitment%20Chain.png?alt=media\&token=0edbbaf0-ba90-45cd-a398-69cda36d8a89)

The State Commitment Chain is considered in order to approve the validity of transactions and it helps us provide a fault-proof condition.

We have a “challenge window” to make sure about commitment proofs within the Layer 2 transactions. The commitments are considered pending for a period of time that is called the “challenge window”.

* If a commitment is considered unchallenged during the time, it’s the final. So, the process of state commitment proof proceeds.
* If a commitment is challenged, it is considered invalidated through “fault proof” and goes to be removed and replaced by another commitment.

According to the diagram, we have 3 processes related to L2 transactions that must be considered to provide a fraud-proof condition and prove the data availability and transaction validity.

* Stage 1: L2 users post transactions directly to the sequencer and it records them in the local blockchain.
* Stage 2: The sequencer periodically posts transactions to the `CanonicalTransactionChain` contract at stage 2. Note that the sequencer then grabs the batch of the user-published transactions located in the `CanonicalTransactionChain` contract.
* Stage 3: The sequencer processes the batch of retrieved transactions and it produces a State Batch and appends the generated Merkle root. The Merkle root is an important part of the process as verifiers need it to verify the validity of the process that occurred recently.

Referring to the 3 stages explained above, the State Commitment part is a very important process in view of security and transaction validity.
