# Actors and Their Roles

An optimistic Ethereum platform relies on a fraud-proof mechanism, and it consists of several sections to achieve a trusted level of the fraud-proof process. It is used to prove the legitimacy of the transactions which are being requested by users. Sequencers, proposers, and verifiers have critical roles in such a fraud-proof process in the blockchain network.

### Sequencers <a href="#_aeub78l1wymw" id="_aeub78l1wymw"></a>

The sequencer is of the essence in rollup systems. It’s a semi-privileged participant and plays a key role in enabling instant transactions. Sequencers receive transaction requests from users, sort them, and then submit transactions on a particular blockchain layer. It is essential to have a sequencer in the optimistic Ethereum blockchain, and it’s responsible for assigning an order to L2 transactions, similar to miners on L1.

A sequencer regularly receives transactions from a user, sorts them, and then mines them out. As a result, a confirmation message is sent to the user so that we can achieve an organized procedure in the case of L2 transactions.

Keep the following facts in mind about the sequencer in the blockchain:

* The sequencer is run by the system creator.
* There is only one sequencer at a time in order to achieve consensus on transactions and extremely rapid transactions.
* Users can send a transaction to the sequencer and get confirmation messages showing that the transaction was successful. This process takes about some seconds and tells you that the transaction will be included in the next rollup batch.
* It is an instant confirmation compared to L1 transactions, so it has weaker security compared to L1 transactions. But, there is stronger security compared to 0-conf L1 transactions.
* If the sequencer’s batch of transactions is confirmed on L1, the security level is the same.

#### Censorship Resistance <a href="#_ghu8ovyvfk87" id="_ghu8ovyvfk87"></a>

In the event that a malicious sequencer censors user transactions, the user should consider these points:

* The user should `enqueue()` the transactions directly to the L1 queue. It will force the sequencer to include the transactions in the L2 chain within the `FORCE_INCLUSION_PERIOD` transaction.
* If any transaction in the queue is older than `FORCE_INCLUSION_PERIOD` blocks, that transaction must be added to the chain before the protocol allows the sequencer to add other transactions.
* In the case the sequencer stops submitting transactions entirely, the protocol allows users to add transactions to the CTC by calling the `appendQueueBatch()` function.

#### The Roles of the Sequencer <a href="#_3ngifo5kqmwh" id="_3ngifo5kqmwh"></a>

The sequencer in optimistic Ethereum has several roles and it can be cloud-based or a blockchain node running in a consensus system. The main roles of a sequencer are as follows:

* The sequencer receives new transactions from users.
* The sequencer must process transactions instantly without delay to determine an optimal ordering according to the situation.
* The sequencer determines the ordering of transactions in the CTC. This process must follow the constraints imposed by the `appendSequencerBatch()` function.
* The sequencer should append transactions from the CTC’s queue within the “Force Inclusion Period”.

### Proposers <a href="#_vy12geso1s6z" id="_vy12geso1s6z"></a>

Proposers evaluate transactions in the Canonical Transaction Chain, and then ‘commit’ to the resulting state by writing them into the State Commitment Chain. For the privilege of this role, the proposers must deposit a bond.

This bond will be noted in the event of a successful fraud-proof process on a state root committed by the proposer.

Note that the proposer is expected to change in the future, but is currently working identically with the sequencer.

#### The Roles of Proposers <a href="#_6bvbiget4n5j" id="_6bvbiget4n5j"></a>

* Proposers process transactions in the CTC and they post new state roots to the SCC.
* Proposers must be collateralized by depositing a bond to the `BondManager`.

### Verifiers <a href="#_n2c0jl1dy834" id="_n2c0jl1dy834"></a>

Verifiers have similar roles to proposers, but you need to consider some differences. A verifier evaluates transactions in the Canonical Transaction Chain in order to determine the resulting state root following each transaction.

In the case of verifying process, note the following facts:

* If a verifier finds an incorrect proposed state root, it will be considered a fraud case. As a result, a reward is earned and taken from the proposer’s bond.

In such a verifying model, multiple accounts can contribute to the fraud-proof process and improve the overall security of the process. So, those accounts can earn the reward.

#### The Roles of Verifiers <a href="#_iekai0smtfql" id="_iekai0smtfql"></a>

* Verifiers read transactions from the CTC. They process transactions and verify the correctness of state roots in the State Commitment Chain.
* If there is an invalid state root, the verifier initiates and completes a fraud-proof task.

### Users <a href="#_vje6pmq5b2gj" id="_vje6pmq5b2gj"></a>

Users have accounts and they are known by their account addresses. Users may request transactions on an optimistic Ethereum network and a blockchain is pointless without existing users.

#### The Roles of Users <a href="#_u7tvipw97zxi" id="_u7tvipw97zxi"></a>

* Users may post L2 transactions via the sequencer’s RPC endpoint. The transactions will be appended to the sequencer's batch of transactions.
* Users may submit an L2 transaction via the Canonical Transaction Chain’s queue on L1. This process can be used to:
  * Circumvent censorship caused by a sequencer in the blockchain.
  * Send a cross domain message from an L1 contract account which checked by MVM\_DiscountOracle.isXDomainSenderAllowed.
