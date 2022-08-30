# Protocol FAQ

## How does Metis Maintain a Constant Uptime?

The system is designed in a way that any malfunctioning actor gets charged and swapped out through the constant cycles of rotation. It is accomplished with the Peer Network which includes Sequencers, Verifiers, and Block Producers.

## What is the Peer Network?

It is a network of decentralized actors (nodes) that serve the Metis system. Anyone can become a part of the Peer Network just by setting up a [node](https://github.com/ericlee42/metis-verifier-node) on their computer. The participants of the Peer Network receive revenue with a portion of the fees from all the transactions that go through the Peer Network.

## Where does Metis Store the Transaction Data?

Metis stores the transaction data in Memolabs storage and the Peer Network.

## If Memolabs goes down, will the System still Operate?

Yes, in this case the Verifier downloads the transaction data from the Peer Network. The peer network will continue to exist as long as there is a single node on the network.

## How does the Metis Smart L2 Achieve Cheaper Transactions?

The Metis Smart L2 posts the minimum required data on Layer 1 to ensure the lowest cost. There are 3 types of data that are always posted to Layer 1:

* The&#x20;



The bundle of transactions are posted by the Sequencer into the Memolabs storage location and the Merkle Tree Transaction Batch Root (MTTBR) gets posted on Layer 1.&#x20;



In an oversimplified analogy, you can think about MTTBR as a bank cheque that you can withdraw the money from. Let's say you want to give someone a big sum of money: in a more efficient way you would give a bank cheque instead of a bag of cash. Metis does the same: instead of posting huge chunks of transaction data to Layer 1, Metis utilizes MTTBR to make it way more efficient while still utilizing the security of Ethereum.
