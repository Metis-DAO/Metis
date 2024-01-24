# Protocol FAQ

## How does the Metis L2 Achieve Cheaper Transactions?

A traditional Optimistic Rollup stores the full transaction information all on Layer 1. The Metis Smart L2 posts the minimum required data on Layer 1 to ensure the lowest cost, storing the transaction data off-chain. The Smart L2 mechanism Being able to bring the data back on-chain at any point, retaining the same security as a Layer 2. There are 2 types of data that are **always** posted to Layer 1 by a Sequencer:

* **Merkle Tree Transaction Batch Root (MTTBR)** - The calculated list of transactions in a specified order.
* **Merkle Tree State Root (MTSR)** - The calculated state hash of the transaction execution.

The off-chain data component is the actual transaction data and is posted by the Sequencer in Memolabs storage. This storage is significantly cheaper to include than on Layer 1 Ethereum.

## How does the Metis L2 Retains the same Security Mechanisms as other Layer 2s?

The off-chain data component is the actual transaction data and is posted by the Sequencer in Memolabs storage. Transaction data can also be accessed by the Peer Network. If the Sequencer does not post the Transaction data to Memolabs, then a Verifier can request that the transaction data that the Sequencer has attested can be posted on-chain.

## What are the Mechanisms in Place to Guarantee that the Network has a Constant Uptime?

The system is designed in a way that any malfunctioning actor gets charged and swapped out through the constant cycles of rotation. It is accomplished with the Peer Network which includes Sequencers, Verifiers, and Block Producers.

## Where does Metis Store the off-chain Transaction Data?

Metis stores the transaction data in [Memolabs](https://www.memolabs.org/) and the Peer Network.

## What is the Peer Network?

It is a network of decentralized actors (nodes) that serve the Metis system. Anyone can become a part of the Peer Network just by setting up a [node](https://github.com/ericlee42/metis-verifier-node). The participants of the Peer Network receive revenue with a portion of the fees from all the transactions that go through the Peer Network.

## If Memolabs goes down, will the System still Operate?

Yes, in this case the Verifier downloads the transaction data from the Peer Network. The Peer Network will continue to exist as long as there is a single node on the network.

## What are the Security Model of the Metis Bridge?

The Metis Bridge is a [Natively Verified](https://blog.connext.network/the-interoperability-trilemma-657c2cf69f17) bridge, which means that it is secured by the native Smart L2 protocol. This means that there are no multisigs or external validators present that could compromise the security of the bridge. The Metis bridge does have a set withdrawal time of 7 days as it is verified Optimistically. For additional details into the process, see the [Metis Smart L2 Explained Section](metis-l2-explained.md)
