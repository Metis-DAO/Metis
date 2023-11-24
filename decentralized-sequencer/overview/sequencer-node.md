# Sequencer node

<figure><img src="https://lh7-us.googleusercontent.com/Oe1xh9x7bWa3LdSdenHA7eIJUAPqaU84qzvYto0IiGu8zklRCdgjyJfwF_jNPY6a2EKielrqi50xheFrTB5JQWB_8O0YVqpF8172XXF5LpVyZaZxZm9zScfDS41FKTZlT5XBRKBNsBH5MhI6561zkA3jrUcJEBel_-tlu5IjSoOd1Y1CFyBLJz5PhCNa0g" alt=""><figcaption></figcaption></figure>

The Sequencer node (or Metis node) includes&#x20;

1. L2 Geth (including the OP-Node)
2. Batch submitter (Proposer)
3. Adapter module

## **L2 Geth (including the OP-Node)**

* It is responsible for transaction sequencing and assembly of the blocks on the Metis layer.
* The processing logic of whether it is the current block sequencer is added in the `applyTransactionToTip` function, to conduct regular sequencer rotation.
* The sequencer (OP-NODE) node obtains the current sequencer position in the rotation list which corresponds to the blockheight with the help of the Adapter module (see below) from the MPC Consensus Layer. It checks whether it is the current sequencer in the rotation –  if so, constructs the batch, if not, does not construct the batch.

The main process is described in the following flowchart:

<figure><img src="https://lh7-us.googleusercontent.com/61XgsYnVIF-XuvcZKlvnK8xT6Zcyy2edqk9Z2qbcSu_zYRVRUIhMOJd47dSFGM5954Ub0vFbktsTbvPGKouCxo18NqdHtjIn01rVDdZfb18DEkD0dStFuhbp6i3n8E-Z_JeEu4lfMuF3meruotmysNC6Y2G3qhI7f3FcVWdsRwAToqYuXLBiqxKLe0owtg" alt=""><figcaption></figcaption></figure>

## **Batch submitter (Proposer):**

* Responsible for building the batches and submitting them to Layer 1 after they get signed by multiple sequencers;
* For decentralization it utilizes MPC – multiple sequencers sign the transaction batch jointly – when the transaction batch is being formed.
* MPC service signs the batch submission with such entry parameters: `batchID`, `signHash`, and the signature result has such parameters: `batchID` – return value: signature r, s, v-values.
* Query the corresponding `signHash` according to the `batchID`: Input parameter: `batchID` – return value: `signHash`

## **Adapter module:**

* Responsible for interacting with the other external modules on the consensus layer (PoS Node).

Additional information:

The core problem of the single-sequencer model adopted by other Layer 2 networks is the the inability to perform sequencer rotation. Here’s how Metis tackles it:&#x20;

* The rotation information is stored on the L2 contract (managed by sequencers, named as `MetisSequencerSet` contract), so that all nodes can obtain the latest sequencer rotation information through L2 transactions, and the management of the sequencer list contract is controlled by the consensus layer (PoS nodes);
* After the consensus layer produces the sequencer list information (contained in each epoch), the list will be signed by MPC, and the current sequencer will initiate a transaction to update the sequencer list;
* Each approved sequencer in the sequencer list can verify the current (picked) sequencer in rotation according to the sequencer list.
* If the current sequencer fails to order transactions within the specified time, or produces wrong transactions (like initiating two transactions with identical L2 TxID), the node is considered malicious. The PoS layer will select a new sequencer (construct `ReselectSeqencer` signature transaction, containing expected TxID, version and other information, signed by the MPC service), and the new sequencer will initiate a `ReselectSeqencer` transaction on the current TxID on L2, writing the information of the new sequencer into `MetisSequencerSet` contract.&#x20;

<figure><img src="https://lh7-us.googleusercontent.com/wAMClYcqsSS-QUKDY5SyKOfIuOj7xWLuFkh1v6kmE4_t63L3RnKrzgfUk7I-R1NNONqUCy8Fw16kdTZxcro1riF0k-rnZrT6Z5FkX99J7bm2xU-GmFJB40GUDyL3TDBw_lrN_6u8L9EuKr5mUKlb6Dq6gLoZlqY4RfhXWjiYAt5-Xd-Rn2ODw5ORC4Jvyw" alt=""><figcaption></figcaption></figure>

* In case of rotation, when a node receives a regular transaction with TXID, it puts on hold the regular transaction and executes MetisSequencerSet.sol contract transaction based on the latest version, then the PoS layer selects the new sequencer that will execute the regular transaction. This ensures that the update to the sequencer list contract is executed, and the execution result reflects the latest state.

<figure><img src="https://lh7-us.googleusercontent.com/n0JheWDSQQrMdA5Q8EFIPt8mpCSwP9-HVYgtDfvCoFc6lfCWEh2i8NNcKUCynWvN67jZNRyqrVWLZQ0ZCnNVMNH_-BY9bhpCC_7Voe6Jhy0bcvv9xPTAvj9EH1GD3i8FRo0iRq6jfoBp9idteWfsxtCSRpVvraZ37vUs-rdpYMhDMnxkBr3-Xwl_WXno7Q" alt=""><figcaption></figcaption></figure>

Main interface:

* `CheckNeedReselectSeq` judges whether Reselect is needed, and checks whether the current sequencer node produces blocks normally
* `GetCurrentSeq` Get the current seq, read from the seq contract
*   Sequencer set contract design:

    `type SeqSetInfo struct {`\
    `startNum int64`\
    `endNum int64`\
    `sequence address`\
    `}`
* `seqList []SeqSetInfo` is used to store `seqList` information:
  * `AddNewSeqSet` : Add seq sets information for the next epoch
  * `ReselectSeq`: Reselect sequencer

