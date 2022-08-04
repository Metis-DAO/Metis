# How to Get Started With the Client Integration

### How to Get Started With the Client Integration? <a href="#_f6ba8tpewniw" id="_f6ba8tpewniw"></a>

We will introduce some pieces of code in this section to help you make use of client integration. In this section, you can find the required functions written in JS or TS used to handle some specific operations within the Metis platform.

Note that all the client integration functions and code examples are for HTTP in JS or TS.

### constructor(appId, accessToken, refreshToken, expiresIn, apiHost?) <a href="#_7q763dnbf9u6" id="_7q763dnbf9u6"></a>

Creating a new instance of the HttpClient.

```
const httpClient = new httpClient(
// The app id found on the application dashboard of Polis
"60f4f7e5ac31e3dccdcdecb5",
//The 0Auth2 Access token of your application
"3e2444385db44f1a802390f692041e92",
// The 0Auth2 refresh token of your application
"3eb18d851e414048b171ff53050b4fa2",
// Seconds until the accessToken expires
1800,
// The API host ip address
"localhost.localdomain"
);
```

| Parameter Name | Parameter Type | Parameter Description                                                 |
| -------------- | -------------- | --------------------------------------------------------------------- |
| appId          | string         | The Id of the specific application on the Polis application dashboard |
| accessToken    | string         | 0Auth2 API access\_token of your specific application                 |
| refreshToken   | string         | The token which refreshes the access\_token                           |
| expiresIn      | number         | Integer of seconds until API access\_token expires                    |
| apiHost        | string         | IP of access                                                          |

### Returns <a href="#_pid9jzm2hgax" id="_pid9jzm2hgax"></a>

Return example for accessing the token and confirming the transaction.

```
// Return Instance Example
HttpClient {
// The access token for the application api
accessToken: "68572b9bac1f499098b507467094662e"
// The API host of the application
apiHost: "https://rocket.metis.io"
// Your Polis app id
appId: "60ee207cac31e30b0b9f59fe"
// The transaction confirmation URL
confirmUrl: "https://rocket.metis.io/#/oauth2/confirm-tx"
// 0Auth2
oAuth2Client: Oauth2Client {
oauthRedirectUrl: "https://rocket.metis.io/#/oauth2-login?",
apiHost: "https://rocket.metis.io",
oauth2User: {…}
}
// The refresh token of application api
refreshToken: "664bbed738ae44469f1edc2be1016637"
// Promise config
swalPromise: null
}
```

### sendTx(domain, chainid, fun, args?, succCallback?, errCallback?) <a href="#_fqp3bf3zz0hm" id="_fqp3bf3zz0hm"></a>

Sending a transaction onto the blockchain and calling a function within the smart contract.

```
// Getting the account balance of a wallet
const balance = httpClient.sendTx(
// This is your contract name when you assigned it to Polis
'my_contract',
// The chain id of blockchain to send the transaction onto
435,
// The specific function that you would like to call within the contract
'balanceOf',
// The parameters to call the contract function with
['0xabcffffffff'],
// Successful callback function passed into this function call
() => console.log("tx sent"),
// Error callback function passed into this function call
() => console.log("tx failed")
);
```

| Parameter Name | Parameter Type | Parameter Description                                                                                         |
| -------------- | -------------- | ------------------------------------------------------------------------------------------------------------- |
| domain         | string         | Deployed smart contract name                                                                                  |
| chainid        | number         | Blockchain's chain ID that your contract is deployed on                                                       |
| fun            | string         | Name of the function to call within the deployed smart contract                                               |
| args           | array          | Parameters to call the function above in order                                                                |
| succCallback?  | function       | The success function which is passed into the sendTx function to be called when execution is successful       |
| errCallback?   | function       | The success function which is passed into the sendTx function to be called when execution results in an error |

### sendTx Return <a href="#_3hqptrrcwull" id="_3hqptrrcwull"></a>

sendTx Return example:

```
{
// Execution status
act: "SUCCESS"
// Parameters passed in at input
args: ["0x6902702BB5678D7361C94441c71F600C255dd833"]
// The chain id of the contract
chainid: 435
// The contract address
contract_address: "0x14B35991f4f2fb275C86156C6DD816b6F7D10052"
// Data status
data: "ok"
// The Domain of the contract on Polis
domain: "testest"
// The address of tx
eth_address: "0x137f2f8406594F855A913574A2e4DEC7E5383384"
// Contract function called
function: "balanceOf"
// Nonce of block containing tx
nonce: 0
// Result returned by the contract function call
result: 0
}
sendTxAsync(domain, chainid, fun, args?)
```

Sending a transaction onto the blockchain asynchronously and calling a method within a smart contract.

```
// Sending tokens
const result = await httpClient.sendTxAsync(
// This is your contract name when you assigned it to Polis
"my_contract",
// The chain id of blockchain to send the transaction
435,
// The specific function that you would like to call within the contract
"sendToken",
// The parameters to call the contract function with
[
"0x11e575e473D552849b35d15A71C85bfFA511f45B",
"0xb253C95DBc5e8E174C2D5272Ca8b06539c25E527",
900
]
);
```

| Parameter Name | Parameter Type | Parameter Description                                           |
| -------------- | -------------- | --------------------------------------------------------------- |
| domain         | string         | Deployed smart contract name                                    |
| chainid        | number         | Blockchain's chain id that your contract is deployed on         |
| fun            | string         | Name of the function that you are calling in the smart contract |
| args           | array          | Parameters that are needed to call the function above in order  |

#### sendTxAsync Return <a href="#_nmwk8xr86que" id="_nmwk8xr86que"></a>

An example of sending a transaction asynchronously.

```
// sendTxAsync Return Instance Example
{
// Result Output
act: "SUCCESS",
// Arguments that were passed in
args: ["0x6902702BB5678D7361C94441c71F600C255dd833"],
// The chain id of tx
chainid: 435,
// The contract address called
contract_address: "0x14B35991f4f2fb275C86156C6DD816b6F7D10052",
// Data status
data: "ok",
// The domain used on Polis
domain: "testest",
// The address of tx
eth_address: "0x137f2f8406594F855A913574A2e4DEC7E5383384",
// The function of contract called
function: "balanceOf",
// Nonce of block
nonce: 0,
// Return value of the contract function called
result: 0,
}beforeConfirm(domain, chainid, address, fun, args?, gas?, gasPrice?, fee?)
```

Checking the final validity of a transaction before it is executed on chain.

```
// Checking ClaimToken transaction
const result = await httpClient.beforeConfirm(
// This is your contract name when you assigned it to Polis
"my_contract",
// The chain id of blockchain to send the transaction onto
435,
// The address of the initiation of the tx
"0x11e575e473D552849b35d15A71C85bfFA511f45B",
// The name the function in the contract which is called
"claimToken",
// The parameters to call the contract function with
[
"0x11e575e473D552849b35d15A71C85bfFA511f45B",
"https://www.humanesociety.org/sites/default/files/styles/1240x698/public/2020-07/kitten-510651.jpg?h=f54c7448&itok=ZhplzyJ9"
],
// The Chain gas limit value
1500000,
// The gas price on chain
100000,
// Fees for this specific transaction
16000000000
)
```

| Parameter Name | Parameter Type | Parameter Description                                          |
| -------------- | -------------- | -------------------------------------------------------------- |
| domain         | string         | Polis domain name                                              |
| chainid        | number         | Blockchain's chain id that you deployed the contract on        |
| address        | string         | Address of the caller of the transaction                       |
| fun            | string         | Name of the smart contract function called in this transaction |
| args           | array          | Parameters that are passed into the call of the function above |
| gas            | string         | The minimum gas for sending the tx on the blockchain           |
| gasPrice       | string         | Gas price of the blockchain for the TX                         |
| fee            | string         | Gas fees for the specific TX                                   |

### queryTx(chainid, tx, succCallback?, errCallback?) <a href="#_9zm37qe3avdl" id="_9zm37qe3avdl"></a>

Querying a transaction’s validity and calling a successful or an error message in execution.

```
// Querying a transaction on the Metis testnet
const result = httpClient.queryTx(
// The chain id of blockchain to send the transaction into
435,
// The transaction hash to query
"0x7a242f5d0b10df0db158fcedb9d19f9d261dfbdc64c837370f1d13d2ad4862a4",
// Inputting an example of successful callback function into the function call
() => console.log("tx is valid"),
// Inputting an example of error callback function into the function call
() => console.log("tx is not valid")
)
```

| Parameter Name | Parameter Type | Parameter Description                                      |
| -------------- | -------------- | ---------------------------------------------------------- |
| chainid        | number         | The blockchain's chain ID that the contract is deployed on |
| tx             | string         | Specific transaction hash                                  |
| succCallback?  | function       | The function that is passed into queryTx to be called      |
| errCallback    | function       | The function that is passed into queryTx to be called      |

#### queryTx Return <a href="#_djgyvkvaratm" id="_djgyvkvaratm"></a>

queryTx Return Example:

```
{
// Successful Query of tx hash
act: "SUCCESS"
// The chain Id of tx query
chainid: 435
// Returned data (nothing)
data: []
// The domain of the contract used on Polis
domain: "mydomain"
// Tx status
status: "SUCCEED"
// The transaction hash on chain
tx: "0x7a242f5d0b10df0db158fcedb9d19f9d261dfbdc64c837370f1d13d2ad4862a4"
}
// Console should also output: "tx is valid" or "tx is not valid" based on input examplequeryTxAsync(chainid, tx)
```

Querying transactions’ validity asynchronously to return a promise.

```
// Querying Transactions Asynchronously
const result = await queryTxAsync(
// The chain Id of the Transaction
435,
// The executed transaction hash on chain
"0xbf09763a4eefd24cf08439d30d82581c366ecfa62763c1ad686a6e8b741f0b8b"
);
```

| Parameter Name | Parameter Type | Parameter Description                            |
| -------------- | -------------- | ------------------------------------------------ |
| chainid        | number         | The chain ID that the transaction is executed on |
| tx             | string         | The transaction hash                             |

#### queryTxAsync Return <a href="#_dew4r7ul9xn" id="_dew4r7ul9xn"></a>

`queryTxAsync` Return Example:

```
{
// Call status
act: "SUCCESS"
// The chain Id with tx
chainid: 435
// Data returned (nothing)
data: []
// The domain name of the contract on Polis
domain: "mydomain"
// Tx status
status: "SUCCEED"
// The Tx hash
tx: "0x7a242f5d0b10df0db158fcedb9d19f9d261dfbdc64c837370f1d13d2ad4862a4"
}
]closeConfirmDialogue()
```

Closing the dialog and calling the close() method without any parameters.

`httpClient.closeConfirmDialogue();`

### error(msg) <a href="#_nqtgrwcz2chw" id="_nqtgrwcz2chw"></a>

Calling the error method and sending an error message to the console.

`httpClient.error("Deployment Error");`

| Parameter Name | Parameter Type | Parameter Description                                 |
| -------------- | -------------- | ----------------------------------------------------- |
| msg            | string         | An error message that will be logged into the console |

#### Error Return <a href="#_igfes69jl23f" id="_igfes69jl23f"></a>

Error return example:

`["socket client runtime error!", "Deployment Error"]`

### handleRefreshToken(callback?) <a href="#_nf9w12vxmeae" id="_nf9w12vxmeae"></a>

Using the `refreshToken` to obtain a new `accessToken`. The callback function should be called and log the message “Successful Refresh” in the console.

```
// Calling the handleRefreshToken with the refreshFunction
httpClient.handleRefreshToken(
// Callback function
() => console.log("Successful Refresh")
);
```

| Parameter Name | Parameter Type | Parameter Description                                 |
| -------------- | -------------- | ----------------------------------------------------- |
| callback?      | function       | A function that is passed into the handleRefreshtoken |

### handleRefreshTokenAsync() <a href="#_svbs8cwnj5rt" id="_svbs8cwnj5rt"></a>

Handling Refresh token asynchronously as input without any parameters.

`await httpClient.handleRefreshTokenAsync()`

### log(obj) <a href="#_twbs3dsz2izj" id="_twbs3dsz2izj"></a>

Logging different variables.

`// Passing string into the log function and logging them into the console`

`httpClient.log(["address: ", "0x11e575e473D552849b35d15A71C85bfFA511f45B"])`

| Parameter Name | Parameter Type | Parameter Description                                  |
| -------------- | -------------- | ------------------------------------------------------ |
| obj            | array          | An array of variables that will be logged into console |

#### Log Return <a href="#_gctky2npx1jh" id="_gctky2npx1jh"></a>

`// Log return example`

`["address: ", "0x11e575e473D552849b35d15A71C85bfFA511f45B"]`

| Return Type | Return Description        |
| ----------- | ------------------------- |
| void        | Does not return any value |

Please feel free to reach out to our [Help Center](https://metisdao.atlassian.net/servicedesk/customer/portals) if you have any technical questions.
