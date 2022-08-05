# The METIS (token) Gateway

### The METIS (token) Gateway <a href="#_kujzp4pgsvkw" id="_kujzp4pgsvkw"></a>

You will need some METIS tokens to pay for transactions on Layer 2. So, moving METIS from L1 to L2 is necessary. The easiest way to move some METIS tokens from L1 to L2 is the METIS Gateway.

### Under the Hood <a href="#_l5ydtjw6l9ki" id="_l5ydtjw6l9ki"></a>

The METIS gateway acts as a utility tool like any other blockchain bridge. It is used to send METIS tokens from L1 to L2 and is made up of 2 primary contracts.

* Layer 1: L1CrossDomainMessenger
* Layer 2: L2CrossDomainMessenger

### Steps for Sending METIS Tokens from Layer 1 to Layer 2 <a href="#_4cqm9i2aeh2l" id="_4cqm9i2aeh2l"></a>

* Step 1: You need to send some METIS tokens to the L1CrossDomainMessenger contract.
* Step 2: That amount of METIS token is locked in the contract.
* Step 3: The L1CrossDomainMessenger contract sends a message to the L2CrossDomainMessenger contract that is on Layer 2.
* Step 4: After a while, the L2CrossDomainMessenger contract notices the message.
* Step 5: The L2CrossDomainMessenger mints some ETH equal to the amount of ETH that was deposited on Layer 1.

### Steps for Withdrawing METIS Tokens from Layer 2 to Layer 1 <a href="#_6guc9sxks5ml" id="_6guc9sxks5ml"></a>

* Step 1: You send a “withdrawal” transaction request to the L2CrossDomainMessenger contract that has been deployed on Layer 2.
* Step 2: The L2CrossDomainMessenger contract burns an equal amount of ETH that you have requested for withdrawal.
* Step 3: The L2CrossDomainMessenger contract sends a message to the L1CrossDomainMessenger contract. It says that the L1CrossDomainMessenger contract should unlock the equal amount of ETH.
* Step 4: You need to wait a couple of days for the fraud-proof process.
* Step 5: At the end, you send a second and final “withdrawal” transaction to the L2CrossDomainMessenger contract and get your funds.
