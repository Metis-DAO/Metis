# Cross Domain Messaging

### Cross Domain Messaging <a href="#_by1x6eih6ukv" id="_by1x6eih6ukv"></a>

Cross Domain Messaging is a great approach to communicating between multiple domains and it allows us to send or receive messages to or from different domains. This section covers the sending and relaying of messages, either from L2 to L1 or from L1 to L2.

Note the following points in the case of Cross Domain Messaging:

* From L2 to L1: Messages are validated by verifying the inclusion of the message data in a mapping of a contract on the L2 state.
* From L1 to L2: Messages are validated simply by checking that the `ovmL1TXORIGIN` matches the expected address.

### Cross Domain Messengers Contracts (xDMs) <a href="#_pe247tzi05ul" id="_pe247tzi05ul"></a>

We have 2 low-level bridge contracts known as the L1 and L2 Cross Domain Messengers. These contracts are paired in the sense that they reference each other’s addresses in order to validate cross domain messages.

### L2 to L1 Messaging Flow <a href="#_jm3uai2b29tk" id="_jm3uai2b29tk"></a>

#### Starting on L2 <a href="#_lrjmv2fbwcbg" id="_lrjmv2fbwcbg"></a>

* Stage 1: A whitelisted account on L2 may call the `L2CrossDomainMessenger.sendMessage()` function with the information for the L1 message (`aka xDomainCalldata`). (i.e. \_target, msg.sender, \_message)
* Stage 2: This data is hashed with the `messageNonce` storage variable and the hash is stored in the `sentMessages` mapping.
* Stage 3: The `messageNonce` is then incremented.
* The L2CrossDomainMessenger contract then passes the `xDomainCallData` to the `OVM_L2ToL1MessagePasser.passMessageToL1()` function. Note that `xDomainCalldata` is hashed with `msg.sender` (i.e. ovmCaller) and written to the `sentMessages` mapping.

#### Proceeding On L1 <a href="#_drtrv8cj0a0w" id="_drtrv8cj0a0w"></a>

* Stage 1: The Relayer may call `L1CrossDomainMessenger.relayMessage()`, providing the raw message inputs and an L2 inclusion proof.
* Stage 2: The validity of the message is confirmed by the following functions:
  * `_verifyStateRootProof()`: This function checks that the fraud-proof window has been closed for the batch in which our transaction belongs to that. The function also checks that the batch is stored in the `ChainStorageContainer`.
  * `_verifyStorageProof()`: This function checks the proof to confirm that the message data provided is in the `OVM_L2ToL1MessagePasser.sentMessages` mapping. The function also checks that our transaction has not already been written to the `successfulMessages` mapping.
* Stage 3: The address of the L2 `ovmCALLER` is then written to the `xdomainMessageSender` state variable.
  * The call is then executed, allowing the target to query the value of the `L1CrossDomainMessenger.xDomainMessageSender` for authorization.
* Stage 4: In the case of a successful condition, it is added to the `successfulMessages` and cannot be relayed again.
* Stage 5: Regardless of the successful condition, an entry is written to the `relayedMessages` mapping.

#### The End <a href="#_ng71ca2phs8b" id="_ng71ca2phs8b"></a>

* The receiver (i.e. `SynthetixBridgeToOptimism`) checks that the caller is the `L1CrossDomainMessenger` and the `xDomainSender` is the `synthetixBridgeToBase` on L2.

### L1 to L2 Messaging Flow <a href="#_15bfxsbok04i" id="_15bfxsbok04i"></a>

#### Starting on L1 <a href="#_o7dhazw7uznz" id="_o7dhazw7uznz"></a>

* Stage 1: Any account may call the `L1xDM’s sendMessage()` function, specifying the details of the call that the L2xDM should make.
* Stage 2: The L1xDM calls `enqueue` on the CTC to add to the transaction queue with the L2xDM as the `target`. Note that the [Transaction.data](https://github.com/MetisProtocol/mvm/blob/develop/specs/protocol/data-structures.md#transaction) field should be ABI encoded to call the `L2CrossDomainMessenger.relayMessage()` function.

#### Proceeding on L2 <a href="#_eyjljhjefnvi" id="_eyjljhjefnvi"></a>

* Stage 1: A transaction will be sent to the `L2CrossDomainMessenger` contract.
* Stage 2: The cross domain message is deemed valid if the `ovmL1TXORIGIN` is in the `L1CrossDomainMessenger` contract. If it is not valid, the execution reverts.
* Stage 3: If the message is valid, the arguments are ABI encoded and keccak256-hashed to `xDomainCalldataHash`.
* Stage 4: The `successfulMessages` mapping is checked to verify that `xDomainCalldataHash` has not already been executed successfully. Note that if an entry is found in `successfulMessages`, execution reverts.
* Stage 5: A check is done to disallow calls to the `OVM_L2ToL1MessagePasser`, which would allow an attacker to spoof a withdrawal. In this stage, execution reverts if the check fails.
  * Future note: The `OVM_L2ToL1MessagePasse`r and this check should be removed in favor of putting the `sentMessages` mapping into the L2xDM.
* Stage 6: The address of the L2 `ovmCALLER` is then written to the `xDomainMessageSender` state variable. The call is then executed, allowing the target to query the value of the `L1CrossDomainMessenger.xDomainMessageSender` for authorization.
* Stage 7: In the case of a successful condition, the message is added to the `successfulMessages`.

Please feel free to reach out to our [Help Center](https://metisdao.atlassian.net/servicedesk/customer/portals) if you have any technical questions.
