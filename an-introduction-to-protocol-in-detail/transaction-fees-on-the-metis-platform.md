# Transaction Fees on the Metis Platform

Transaction fees on the Metis platform are a little different from the Ethereum network. There is no difference from the end-user view, but you must consider some differences as a developer.

There are 3 primary actions when interacting with the Metis platform in the case of transaction fees.

* Layer 2 transactions
* Layer 1 to Layer 2 transactions
* Layer 2 to Layer 1 transactions

### Fees for L2 Transactions <a href="#_6lv50cgqoeb" id="_6lv50cgqoeb"></a>

Fees on Metis are denominated in METIS tokens. The formula is the same as what you see on the Ethereum platform.

* _Fee = transaction.gasPrice \* gasUsed_

You should use eth\_gasPrice to determine the appropriate gas price needed. Also, eth\_estimateGas is used to find an appropriate gas limit.

* Note that if the _transaction.gasPrice_ is too low, your transaction will be rejected.
* If you supply a _transaction.gasLimit_ less than the value of _eth\_estimateGas_, your transaction will be rejected.

#### Metis Gas Estimation Method <a href="#_pt4sjuw53aym" id="_pt4sjuw53aym"></a>

As a developer, you must know the gas estimation method. eth\_estimateGas will return:

* _estimate = (rollupTransactionSize \* dataPrice) + (gasUsed \* executionPrice)_

Where:

* rollupTransactionSize is the size (in bytes) of the serialized transaction that will be published to Layer 1.
* dataPrice is a variable that reflects the current cost of publishing data to Layer 1.
* gasUsed is the standard result of eth\_estimateGas for a transaction.
* executionPrice is a variable that reflects the current congestion level on Layer 2, much like the gasPrice on Layer 1.

### Fees for Layer 1 to Layer 2 Transactions <a href="#_rgdby0nkjvvu" id="_rgdby0nkjvvu"></a>

When you create an L1 to L2 transaction, you only need to pay for the standard Ethereum gas cost. The gas costs are associated with the transaction you are going to create. The USD-denominated cost of the transaction depends on some specific factors, including the level of the current L1 congestion and the current ETH price. It can be between $15 and $75.

Furthermore, the measurement may vary according to additional contract logic and transactions that are performed before the L1-L2 transaction.

### Fees for Layer 2 to Layer 1 Transactions <a href="#_7xfs38io3e3p" id="_7xfs38io3e3p"></a>

Layer 2 to Layer 1 transactions tend to be a little more expensive than Layer 1 to Layer 2 transactions. It’s because you need to create a transaction on both L2 and L1 in this type of bridging.

Please consider the following details when you make a transaction from L2 to L1:

* A Layer 2 transaction that initiates the transaction.
* A Layer 1 transaction that finalizes the transaction.

The cost of the initialization transaction is determined in the same way as any other L2 transaction. The cost of the finalization transaction depends on the current L1 congestion level and the ETH price. In terms of gas, finalization transactions can currently cost upwards of 4-500k gas because they involve verifying a[ Merkle trie](https://eth.wiki/fundamentals/patricia-tree) inclusion proof.
