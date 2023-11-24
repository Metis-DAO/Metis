# MPC module

Multi Party Computation (MPC) module is responsible for the management of the entire life-cycle of the multisignature keys. Conducts external operations such as&#x20;

1. Multisig generation
2. Key resharing
3. Applying the signature
4. Deletion of signature
5. Provides support for the asynchronous usage of many multisignatures

#### Core method processing flow

`keyGen` process flow

Phase 1: Notifying MPC Nodes to Prepare

1. Generate a random `sessionID` locally;
2. Broadcast the `keyGenPrepare` message to all MPC nodes using the p2p network;
3. Upon receiving the `keyGenPrepare` message, each MPC node starts its processing goroutine;
4. Check the local data (the data means whether the TSS module has stored the mpc information corresponding to the id) based on the `keyId`;
5. If there is existing data in the READY state, return the data from storage directly. No need to proceed with the `keyGen` operation;
6. If there is existing data with a PENDING state, return an error to avoid inconsistencies in key generation due to concurrent execution of different key generation calls;
7. Establish a p2p communication channel;
8. Return the `keyGenReady` message to the initiating node;

Phase 2: Initiating the `keyGen` process

1. The initiating node waits to receive `keyGenReady` messages from all nodes;
2. Once the initiating node receives `keyGenReady` messages from all nodes, it broadcasts the `keyGenStart` message to all MPC nodes using the p2p network;
3. Upon receiving the `keyGenStart` message, each MPC node:
   * Constructs a `LocalParty` instance locally;
   * Begins receiving information from other nodes;

#### Additional processes flow

1. In essence, the processing flow of keySign is similar to keyGen, with the difference lying in some data transmission and verification..
2. On the other hand, keyDelete does not involve any tss-lib operations. It only requires broadcasting the KeyDeleteMessage message to all nodes to request the deletion of the key.



