# Metis Smart L2 Explained

## **Actors**

There are 7 distinguished actors participating in the system (Governance Protocol is a part of L1, but it handles a number of functions that makes it a separate entity):

1. **User** - Sends the transactions.
2. **Block Producer** - Responsible for correcting the blockchain, propagating the blocks through the Peer Network.
3. **Memolabs** - Stores the transaction data **** off-chain.
4. **Sequencer** - Responsible for providing transaction data and the data batch to other entities from the Smart L2.
5. **Verifier** - The counterpart of a Sequencer, mostly responsible to keep an eye on Sequencer to not provide false/invalid data;
6. **Layer 1** - The set of smart contracts on Ethereum that handle the security of the system, solves the disputes between Sequencer and Verifier.
7. **Governance Protocol** - Responsible for anything related to the efficiency of the system.

## Definitions

**Merkle Tree Transaction Batch Root (MTTBR)** - The calculated list of transactions in a specified order.

**Merkle Tree State Root (MTSR)** - The calculated state hash of the transaction execution.

## **Process** <a href="#_oamnefrlujtz" id="_oamnefrlujtz"></a>

The entire structure of Metis Smart L2 is designed around several process loops which are designated to mitigate the potential damage and filter the possible malfunctions of decentralized actors and/or other outer ill-wishers.

Here we describe the process of executing and securing transactions on Metis' Smart Layer 2. The desirable end is at section **11.A.3.A.1**

* **1** - The User submits a transaction to the **** Block Producer.
* **2** - The Block Producer produces the blocks for the Smart L2;
  * **2.1** - The Block Producer propagates the blocks through the Peer Network[**\***](metis-smart-l2-explained.md#what-if-the-block-producer-unites-with-sequencer-to-stop-the-system):
* **3** - The User gets confirmation of the transaction from the Smart L2.
* **4** - The Sequencer receives the full transaction data from the Peer Network.
* **5** - The Sequencer computes the data batch (MTTBR and MTSR).
* **6** - The Sequencer submits the full transaction data to Memolabs, from where it will be provided to the Verifier. Memolabs provides the transaction data to any node that is looking to access the transaction data, located by the MTTBR.
* **7** - The Sequencer submits the batch of data to Layer 1 (MTTBR and MTSR).
* **8** - The Verifier attempts to download the full transaction data from Memolabs by retrieving the MTTBR on Layer 1 that was submitted by the Sequencer;
* _If the Memolabs data is NOT available_[_\*_](metis-smart-l2-explained.md#how-do-you-deal-with-the-data-availability-problem-speaker-listener-dilemma)_:_
  * **8.1.1** - The Verifier downloads the full transaction data from the Peer Network.
* **9** - The Verifier computes its own MTTBR from the full transaction data it received from Memolabs / Peer Network.
* **10** - The Verifier downloads the Sequencer’s MTTBR from Layer 1.
* **11** - The Verifier compares its own MTTBR with the MTTBR submitted to Layer 1 by the Sequencer;
* _If the Verifier’s_ MTTBR _= Sequencer’s_ MTTBR_:_
  * **11.A.1** - The Verifier computes its own MTSR from the full transaction data it received from Memolabs / Peer Network;
  * **11.A.2** - The Verifier downloads the Sequencer’s MTSR from Layer 1 submitted by the Sequencer;
  * **11.A.3** - The Verifier compares its own calculated MTSR with the Sequencer’s MTSR;
  * _If the Verifier’s MTSR = Sequencer’s MTSR:_
    * **11.A.3.A.1** - The transactions and state are valid.
  * _If the Verifier’s MTSR =/= Sequencer’s MTSR:_
    * **11.A.3.B.1** - The Verifier submits the transaction data it received from Memolabs / Peer Network to Layer 1 and marks them challengeable;
    * **11.A.3.B.2** - The Verifier can submit a Fraud Proof. The Fraud proofing process will proceed just as a regular Optimistic Rollup hereafter.
* _If the Verifier’s_ MTTBR _=/= Sequencer’s_ MTTBR[\*](metis-smart-l2-explained.md#griefing)_:_
  * **11.B.1** - The Verifier requests the Sequencer via fee payment to post the Sequencer’s transaction data on Layer 1. This fee payment is collected on Layer 1 as an upfront amount to pay for the request fees. The fees are sent to the Governance Protocol to pay for potential reimbursements caused due to griefing. The Sequencer will have a time window (in # of blocks, currently set at around 24 hours) to respond and is responsible for the resulting gas fee. Both the Sequencer and Verifier lose money in this process;
  * **11.B.2** - The System enters the insecure transaction state for the data availability request time window until the next rotation. During this time the Verifier requests the Sequencer to provide valid transaction batch data;
  * _If the Sequencer submits the valid full transaction batch data within the_ data availability request time window_:_
    * **11.B.2.A.1** - The System enters the secure transaction state.
    * **11.B.2.A.2** - The Fraud proofing process will proceed just as a regular Optimistic Rollup hereafter.
  * _If the Sequencer does NOT submit the valid transaction data within the_ data availability request time window_:_
    * **11.B.2.B.1** - The Malfunctioning Sequencer may get kicked and/or slashed by Layer 1;
    * **11.B.2.B.2** - The MTTBR and MTSR that the Sequencer submitted gets marked invalid by Layer 1;
    * **11.B.2.B.3** - The Sequencer gets rotated by Layer 1.
    * **11.B.2.B.4** - The System enters the secure transaction state.

## **How do you deal with the Data Availability Problem / Speaker-Listener Dilemma?**

### Game Theory Mechanics

Both the Sequencers and Verifiers benefit from acting in the best interest of the network. The Sequencer would retain the fees that they would make from processing the transactions normally without withholding data. The Verifiers would benefit by not losing money by making needless data requests. Based on these mechanics, there is no direct guaranteed incentive by attacking the network.

### Griefing

Griefing is the process of needlessly requesting data to be on-chain. It is done as a malicious action towards the Verifier or the Sequencer, where both parties lose monetary value.

#### If the Sequencer is griefing data requests to the Verifiers

This occurs when the Sequencer does not submit data to Memolabs and the Peer Network transaction data does not match what was posted on Layer 1. Since data is not available and cannot be retrieved, the only way to do so would be to request the data directly from the Sequencer. If the Sequencer provides it, and it turns out to be valid, then no direct punishment can occur (For the full flow, see **11.B.1**).&#x20;

**This is a serious attack vector and can affect network security** since the Sequencer can force all Verifiers to pay for transaction data to needlessly be brought on-chain. In the case when there are no Verifiers remaining because of attrition (fees), the Sequencer can submit a falsified MTSR and withdraw funds from the network after the 7-day withdrawal period. There are 2 flows to prevent this from happening:

_The Rotation Flow_

* The Sequencer checks if it is selected by Layer 1 for the current rotation;
  * _If the Sequencer is the current selected Sequencer:_
    * The Sequencer continues with the regular flow, in this case they download the transaction data from the Peer Network.
  * _If the Sequencer is NOT the current selected Sequencer:_
    * The Sequencer waits for the selection into the next slot on Layer 1.

_The Governance Flow_

* A proposal is submitted to the Governance Protocol to remove the Sequencer;
* The affected Verifiers can get compensated by any lost transaction fees through the data requests made by the Sequencer;
* If the Governance Protocol succeeds, the Sequencer gets removed from the Sequencer pool. The Sequencer will also lose a portion of their funds that are present in Layer 1 to pay for the affected data requests that the Verifiers made;
* A Sequencer rotation is executed.

Using a rotated Sequencer would enable network resilience, as the Sequencer has a limited amount of transactions that it can process per slot. Because of the automatic Sequencer rotation, the damage that a single Sequencer can produce is minimized per slot. In combination with the Protocol Governance, the malicious Sequencer can be manually removed from the Sequencer Pool to prevent further damage to the network.

#### **I**f the Verifier is griefing data requests to the Sequencer

This occurs when the data is present on Memolabs or the Peer Network transaction data matches what was posted on Layer 1, but the needless request is sent to the Sequencer for execution. This is significantly less serious than a griefing attack done by the Sequencer, as it only affects the efficiency of the network. **This griefing attack does NOT affect the security of the network** and the worst-case scenario is that there would no longer be any Sequencers on the network to process transactions, halting the network and freezing the funds until a new Sequencer is rotated. Since this only affects the efficiency of the platform, a governance flow is included to prevent this attack:

* A proposal is submitted to the Governance Protocol to remove the Verifier;
* The Sequencer can get compensated by any lost transaction fees through the data requests made by the Verifier.
* The Verifier gets banned from making requests to the Sequencer. The Verifier will also lose any funds that are present in Layer 1. There is no rotation needed since other Verifiers work asynchronously.

### **What is the “Insecure transaction state of the system”?**

It is a special state when the system stops in some sense, because the security of transactions after the problematic batch can not be guaranteed. The System will get out of the insecure transaction state after the data availability request time window (currently set at 24 hours) or until the Sequencer provides the valid data.

## **Special Scenarios FAQ** <a href="#_sghwxg6kz4gu" id="_sghwxg6kz4gu"></a>

### What is the process for Sequencer Rotation?

The Sequencer would go out of rotation if the amount of transactions that the Sequencer has posted would be greater than 3000. The other times are due to malicious activities, where the Sequencer would be rotated out as a result of incorrect transaction ordering, data unavaliability, or invalid state calculation.

### What happens if the order of transactions created by the Block Producer differ from what the Sequencer has posted to Layer 1?

* **1** - The Block Producer downloads the Sequencer's MTTBR it sent in the batch to Layer 1;
* **2** - The Block Producer downloads the full transaction data from the Peer Network;
* **3** - The Block Producer computes the MTTBR from the transaction data;
* **4** - The Block Producer compares its own MTTBR with the Sequencer’s MTTBR;
* _If the Block Producer’s MTTBR = Sequencer’s MTTBR:_
  * **4.A.1** - The transactions are verified to be in the correct order.
* _If the Block Producer’s MTTBR =/= the Sequencer’s MTTBR:_
  * **4.B.1** - The Block Producer reorganizes the Smart L2 to match what the Sequencer has submitted to Layer 1;&#x20;
  * **4.B.2** - Block Producer sends a proposal to Governance Protocol to kick the malfunctioning Sequencer;
  * **4.B.3** - If the current Sequencer is the one who malfunctioned, then it gets rotated by Layer 1 (even if the 3000 transaction limit wasn’t fulfilled). If not, then nothing happens.

### **What if the Block Producer unites with Sequencer to stop the system?**

When this happens, the Block Producer stops producing blocks to the Metis Smart L2. The Sequencer may submit the fake transaction and fake state roots to Layer 1, which would prove its malfunction. The community may submit a proposal to the Governance Protocol to punish the Block Producer, and Layer 1 will punish the Sequencer depending on whether they are united or it is only the Block Producer that malfunctioned. Then the system resumes after rotation. The process is shown below:

* **1** - The Smart L2 stops since it does not receive the blocks from the Block Producer.
* **2** - The User gets notified that the transaction has failed.
* **3** - The Sequencer may post an invalid batch (Merkle Tree Transaction Batch Root / Merkle Tree State Root);
* _If the Sequencer does NOT submit the transaction batch data:_&#x20;
  * **3.A.1** - The community (can be Sequencers / Verifiers / Block Producers) submits a proposal to the Governance Protocol to punish the Block Producer;
  * **3.A.2** - The Block Producer may get kicked by Layer 1 if the proposal passes;
  * **3.A.3** - Block Producer gets rotated by Layer 1;
  * **3.A.4** - The Smart L2 gets re-enabled. User has to resend the transaction.
* _If the Sequencer submits a MTTBR and MTSR:_
  * **3.B.1** - The Verifier requests the Sequencer to post the transaction data on Layer 1 which may prove Sequencer’s malfunction. Both Sequencer and Block Producer may get kicked and rotated;
  * **3.B.2** - The System enters the insecure transaction state for the data availability request window or until the Sequencer posts valid transaction data on Layer 1;
  * _If **** the Sequencer submits the valid transaction data within the data availability request time window:_
    * **3.B.2.A.1** - Layer 1 computes a batch from the newly posted transaction data that was submitted by the Sequencer;
    * **3.B.2.A.2** - Layer 1 compares the computed MTSR and the Sequencer submitted MTSR;
    * _If the computed MTSR = submitted MTSR:_
      * **3.B.2.A.2.A.1** - The Block Producer gets rotated by Layer 1;
      * **3.B.2.A.2.A.2** - The Smart L2 gets re-enabled. User has to resend the transaction.
    * _If the computed MTSR =/= submitted MTSR:_
      * **3.B.2.A.2.B.1** - The transaction data that the Sequencer submitted gets marked invalid by Layer 1;
      * **3.B.2.A.2.B.2** - The Sequencer gets rotated by Layer 1;
      * **3.B.2.A.2.B.3** - The Block Producer gets rotated by Layer 1;
      * **3.B.2.A.2.B.4** - The Smart L2 gets re-enabled. User has to resend the transaction.
  * _If the Sequencer does NOT submit the valid transaction data within the data availability request time window:_
    * **3.B.2.B.1** - The MTTBR and MTSR that the Sequencer submitted gets marked invalid by Layer 1;
    * **3.B.2.B.2** - The Sequencer gets rotated by Layer 1;
    * **3.B.2.B.3** - The Block Producer gets rotated by Layer 1;
    * **3.B.2.B.4** - The Smart L2 gets re-enabled. The User has to resend the transaction.

## **Diagrams** <a href="#_riv4fxyvrlil" id="_riv4fxyvrlil"></a>

<figure><img src="../.gitbook/assets/Metis Smart L2 Flowchart (1).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://lucid.app/lucidspark/8c0b52b0-71ae-4ac2-ab6e-17eeab0965c7/edit?viewport_loc=-1712%2C-1727%2C15112%2C7758%2C0_0&invitationId=inv_0fab1530-41c8-4878-b76c-3f9293512b7b" %}
