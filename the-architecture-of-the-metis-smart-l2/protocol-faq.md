# Protocol FAQ

## How does Metis Maintain a Constant Uptime?

The system is designed in a way that any malfunctioning actor gets charged and swapped out through the constant cycles of rotation. It is accomplished with the Peer Network which includes Sequencers, Verifiers and Block Producers (this entity is trusted for now). After the next major upgrade, there will be no more downtime at all, and the maintenance schedule will be controlled by the community through the governance protocol.

## What is the Peer Network?

It is a network of decentralized actors (nodes) that serve the Metis system. Anyone can become a part of the Peer Network just by setting up a node on their computer. The participants of the Peer Network receive revenue with a portion of the fees from all the transactions that go through the Peer Network.

## How does Metis Achieve Cheaper Transactions?

Metis utilizes Merkle Tree (MTSR). In an oversimplified analogy, you can think about MTSR as a bank cheque that you can withdraw the money from. Let's say you want to give someone a big sum of money: in a more efficient way you would give a bank cheque instead of a bag of cash. Metis does the same: instead of posting huge chunks of transaction data to Layer 1, Metis utilizes MTSR to make it way more efficient while still utilizing the security of Ethereum.

## What will happen with my money if I send a Transaction and the Smart L2 gets stopped?

Your money will either successfully reach the desired destination address or you will have to resend the transaction. No money will be lost.

## Where does Metis Store the Transaction Data?

Metis stores the transaction data in Memolabs storage, which is based on blockchain technology to provide safe, efficient and large-scale decentralized cloud storage services.

## If Memolabs goes down, will the System still Operate?

Yes, in this case the Verifier downloads the transaction data from the Peer Network. If the Peer Network data does not match the data that is present on Layer 1, then the Verifier requests Sequencer to post the transaction data on Layer 1 and then downloads it from there.
