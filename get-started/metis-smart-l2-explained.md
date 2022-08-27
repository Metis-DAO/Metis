# Metis Smart L2 Explained

## **Actors**

There are 7 distinguished actors participating in the system (Governance Protocol is a part of L1, but it handles a number of functions that makes it a separate entity):

1. **User** - Sends the transactions.
2. **Block Producer** - Responsible for correcting the blockchain, propagating the blocks through the Peer Network.
3. **Memolabs** - Stores the transaction data.
4. **Sequencer** - Responsible for providing transaction data and the data batch to other entities from the Smart L2.
5. **Verifier** - The counterpart of a Sequencer, mostly responsible to keep an eye on Sequencer to not provide false/invalid data;
6. **Layer 1** - The set of smart contracts on Ethereum that handle the security of the system, solves the disputes between Sequencer and Verifier.
7. **Governance Protocol** - Responsible for anything related to the efficiency of the system.

## Definitions

**Merkle Tree Transaction Batch Root (MTTBR)** - The calculated list of transactions in a specified order.

**Merkle Tree State Root (MTSR)** - The calculated state hash of the transaction execution.

**Memolabs Transaction Storage Location (MTSL)** - The content based address of the submitted data to Memolabs that is sent by the Sequencer.

## **Process** <a href="#_oamnefrlujtz" id="_oamnefrlujtz"></a>

The entire structure of Metis Smart L2 is designed around several process loops (sections 2, 4, 7, 8, 12) which are designated to mitigate the potential damage and filter the possible malfunctions of decentralized actors and/or other outer ill-wishers.

Here we describe the process of executing and securing transactions on Metis' Smart Layer 2. The desirable end is at section **10.A.3.A.1**

* **1** - The User submits a transaction to the **** Block Producer.
* **2** - The Block Producer produces the blocks for Metis Blockchain;
  * **2.1** - The Block Producer propagates the blocks through the Peer Network[**\***](metis-smart-l2-explained.md#what-if-the-block-producer-unites-with-sequencer-to-stop-the-system):
* **3** - The User gets confirmation of the transaction from the Smart L2.
* **4** - The Sequencer computes the data batch (MTTBR and MTSR).
* **5** - The Sequencer submits the full transaction data to Memolabs, from where it will be provided to the Verifier. Memolabs provides the transaction data.
* **6** - The Sequencer submits the batch of data to Layer 1 (MTSL, MTTBR and MTSR).
* **7** - The Verifier downloads the full transaction data from Memolabs by retrieving the MTSL on Layer 1 that was submitted by the Sequencer;
* _**If the Memolabs data is NOT available**_[_**\***_](metis-smart-l2-explained.md#how-do-you-deal-with-the-data-availability-problem)_**:**_
  * **7.A.1** - The Verifier downloads the full transaction data from the Peer Network.
* **8** - The Verifier computes its own MTTBR from the full transaction data it received from Memolabs / Peer Network.
* **9** - The Verifier downloads the Sequencer’s MTTBR from Layer 1.
* **10** - The Verifier compares its own MTTBR with the MTTBR submitted to Layer 1 by the Sequencer;
* _**If the Verifier’s**_** MTTBR **_**= Sequencer’s**_** MTTBR**_**:**_
  * **10.A.1** - The Verifier computes its own MTSR from the full transaction data it received from Memolabs / Peer Network;
  * **10.A.2** - The Verifier downloads the Sequencer’s MTSR from Layer 1 submitted by the Sequencer;
  * **10.A.3** - The Verifier compares its own calculated MTSR with the Sequencer’s MTSR;
  * _**If the Verifier’s**_** MTSR **_**= Sequencer’s**_** MTSR:**
    * **10.A.3.A.1** - The transactions and state are valid.
  * _**If the Verifier’s**_** MTSR **_**=/= Sequencer’s**_** MTSR**_**:**_
    * **10.A.3.B.1** - The Verifier submits the transaction data it received from Memolabs / Peer Network to Layer 1 and marks them challengeable;
    * **10.A.3.B.2** - The Verifier can submit a Fraud Proof. The Fraud proofing process will proceed just as a regular Optimistic Rollup hereafter.
* _**If the Verifier’s**_** MTTBR **_**=/= Sequencer’s**_** MTTBR**[**\***](metis-smart-l2-explained.md#griefing)_**:**_
  * **10.B.1** - The Verifier requests the Sequencer via fee payment to post the Sequencer’s transaction data on Layer 1. This fee payment is collected on Layer 1 as an upfront amount to pay for the request fees. The fees are sent to the Governance Protocol to pay for potential reimbursements caused to to griefing. The Sequencer will have a time window (in # of blocks, currently set at around 24 hours) to respond and is responsible for the resulting gas fee. Both the Sequencer and Verifier lose money in this process;
  * **10.B.2** - The System enters the insecure transaction state for data availability request time window until the next rotation. During this time the Verifier requests the Sequencer to provide valid transaction batch data;
  * _**If the Sequencer submits the valid full transaction batch data within the**_** data availability request time window**_**:**_&#x20;
    * **10.B.2.A.1** - The Fraud proofing process will proceed just as a regular Optimistic Rollup hereafter.
  * _**If the Sequencer does NOT submit the valid transaction data within the**_** data availability request time window**_**:**_
    * **10.B.2.B.1** - The Malfunctioning Sequencer may get kicked and/or slashed by Layer 1;
    * **10.B.2.B.2** - The MTTBR and MTSR that the Sequencer submitted gets marked invalid by Layer 1;
    * **10.B.2.B.3** - The Sequencer gets rotated by Layer 1.

## **How do you deal with the Data Availability Problem?**

### Griefing

Griefing is the process of needlessly requesting data to be on-chain. It is done as a malicious action towards the Verifier or the Sequencer, where both parties lose monetary value.

#### **I**f the Verifier is griefing the data requests to the Sequencer

This occurs when the data is present on Memolabs or the Peer Network transaction data matches what was posted on Layer 1, but the needless request is sent to the Sequencer for execution.

* The Sequencer sends proposal to Governance Protocol to ban Verifier and get compensated;
* The Sequencer can get compensated by any lost transaction fees through the data requests made by the Verifier.
* The Verifier gets banned from making requests to the Sequencer. The Verifier will also lose any funds that are present in Layer 1. There is no rotation needed since other Verifiers work asynchronously.

#### If the Sequencer is griefing the data requests to the Verifiers

This occurs when the Sequencer does not submit data to Memolabs and the Peer Network transaction data does not match what was posted on Layer 1.

* The Verifier sends proposal to Governance Protocol to ban the Sequencer and get compensated;
* The affected Verifiers can get compensated by any lost transaction fees through the data requests made by the Sequencer.
* The Sequencer gets banned from making requests to the Sequencer. The Sequencer will also lose a portion of their funds that are present in Layer 1.&#x20;
* A Sequencer Rotation is made.

### **What is the “Insecure transaction state of the system”?**

It is a special state when the system stops in some sense, because the security of transactions after the problematic batch can not be guaranteed. System enters insecure transaction state for data availability request time window until the next rotation. During this time window, if the Sequencer provides the valid data, the system will get out of an insecure transaction state.\
For now, the duration of data availability request time window is 24 hours;

## **Special Scenarios FAQ** <a href="#_sghwxg6kz4gu" id="_sghwxg6kz4gu"></a>

### What is the process for Sequencer Rotation?

The Sequencer would go out of rotation if the amount of transactions that the Sequencer has posted would be greater than 3000. The other times are due to malicious activities, where the Sequencer would be rotated out as a result of incorrect transaction ordering, data unavaliability, or invalid state calculation.

### What happens if the order of transactions created by the Block Producer differ from what the Sequencer has posted to Layer 1?

* The Block Producer downloads the Sequencer's MTTBR it sent in the batch to Layer 1;
  * The Block Producer downloads the full transaction data from the Peer Network;
  * The Block Producer computes the MTTBR from the transaction data;
  * The Block Producer compares its own MTTBR with the Sequencer’s MTTBR;
  * _**If the Block Producer’s**_** MTTBR **_**= Sequencer’s**_** MTTBR**_**:**_
    * The transactions are verified to be in the correct order.
  * **If the Block Producer’s MTTBR =/= the Sequencer’s MTTBR:**
    * The Block Producer reorganizes the Smart L2 to match what the Sequencer has submitted to Layer 1;&#x20;
    * Block Producer sends a proposal to Governance Protocol to kick the malfunctioning Sequencer;
    * If the current Sequencer is the one who malfunctioned, then it gets rotated by Layer 1 (even if the 3000 transaction limit wasn’t fulfilled). If not, then nothing happens.

### **What if the Block Producer unites with Sequencer to stop the system?**

When this happens, the Block Producer stops producing blocks to the Metis Smart L2. The Sequencer may submit the fake transaction and fake state roots to Layer 1, which would prove its malfunction. The community may submit a proposal to the Governance Protocol to punish the Block Producer, and Layer 1 will punish the Sequencer depending on whether they are united or it is only the Block Producer that malfunctioned. Then the system resumes after rotation. The process is shown below:

* **2.1.A.1** - The Smart L2 stops since it does not receive the blocks from the Block Producer;
* **2.1.A.2** - The User gets notified that the transaction has failed;
* **2.1.A.3** - The Sequencer may post an invalid batch (Merkle Tree Transaction Batch Root / Merkle Tree State Root);
* _**If the Sequencer does NOT submit transaction batch data:**_&#x20;
  * **2.1.A.3.A.1** - The Verifier cannot request the transaction data. Only the Block Producer may get kicked and rotated. In this case, the community (can be Sequencers / Verifiers / Block Producers) submits a proposal to the Governance Protocol to punish the Block Producer;
  * **2.1.A.3.A.2** - The Block Producer may get kicked by Layer 1 if the proposal passes;
  * **2.1.A.3.A.3** - Block Producer gets rotated by Layer 1;
  * **2.1.A.3.A.4** - The Smart L2 gets re-enabled. User has to resend the transaction.
* _**If the Sequencer submits a**_** MTTBR and MTSR**_**:**_
  * **2.1.A.3.B.1** - The Verifier requests the Sequencer to post transaction data on Layer 1 which may prove Sequencer’s malfunction. Both Sequencer and Block Producer may get kicked and rotated;
  * **2.1.A.3.B.2** - The System enters the insecure transaction state for 24 hours until the next rotation or until the Sequencer posts valid transaction data on Layer 1. Invalid data will not be accepted:
  * _**In the case where the Sequencer submits the valid transaction data within the**_** data availability request time window**_**:**_
    * **2.1.A.3.B.2.A.1** - Layer 1 computes a batch from the newly posted transaction data that was submitted by the Sequencer;
    * **2.1.A.3.B.2.A.2** - Layer 1 compares the computed MTSR and the Sequencer submitted MTSR;
    * _**If the computed**_** MTSR **_**= submitted**_** MTSR**_**:**_
      * **2.1.A.3.B.2.A.2.A.1** - The Smart L2 goes out of insecure transaction state;
      * **2.1.A.3.B.2.A.2.A.2** - The Block Producer gets rotated by Layer 1;
      * **2.1.A.3.B.2.A.2.A.3** - The Smart L2 gets re-enabled. User has to resend the transaction.
    * _**If the computed**_** MTSR **_**=/= submitted**_** MTSR**_**:**_
      * **2.1.A.3.B.2.A.2.B.1** - The malfunctioning Sequencer may get kicked and/or slashed by Layer 1;
      * **2.1.A.3.B.2.A.2.B.2** - The fake transaction data that the Sequencer submitted gets marked invalid by Layer 1;
      * **2.1.A.3.B.2.A.2.B.3** - The Sequencer gets rotated by Layer 1;
      * **2.1.A.3.B.2.A.2.B.4** - The Block Producer gets rotated by Layer 1;
      * **2.1.A.3.B.2.A.2.B.5** - The Smart L2 gets re-enabled. User has to resend the transaction.
  * _**If the Sequencer does NOT submit the valid transaction data within the**_** data availability request time window**_**:**_
    * **2.1.A.3.A.3.B.1** - The Malfunctioning Sequencer may get kicked and/or slashed by Layer 1;
    * **2.1.A.3.A.3.B.2** - The MTTBR and MTSR that the Sequencer submitted gets marked invalid by Layer 1;
    * **2.1.A.3.A.3.B.3** - The Sequencer gets rotated by Layer 1.
    * **2.1.A.3.A.3.B.4** - The Block Producer gets rotated by Layer 1;
    * **2.1.A.3.A.3.B.5** - The Smart L2 gets re-enabled. The User has to resend the transaction.

## **Diagrams** <a href="#_riv4fxyvrlil" id="_riv4fxyvrlil"></a>

Coming soon
