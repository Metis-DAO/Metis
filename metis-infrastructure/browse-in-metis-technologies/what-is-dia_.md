# What is DIA

### What is DIA? <a href="#_8j8ip8a3ktpy" id="_8j8ip8a3ktpy"></a>

Trusted data feeds are crucial for building Web 3.0 dapps, and DIA’s mission is to provide these data feeds. DIA ensures dapp developers can get trusted data from external data sources and use it in their projects.

Decentralized Information Asset (DIA) allows you to develop applications and sources, and it can be called a bridge and verifier between off-chain data and on-chain smart contracts.

There is a wide range of features when you use DIA, and you can get tailored data feeds with complete customizations. [DIA data feeds](https://www.diadata.org/) can be used by any network and infrastructure, such as the Metis platform. You can read more about DIA and Metis integrations by exploring the [DIA’s Medium page](https://medium.com/dia-insights/partnership-with-metis-7c0fa3170343).

### Metis DIA Oracle <a href="#_v2w0v1nn9ivu" id="_v2w0v1nn9ivu"></a>

It’s a straightforward method to deploy Metis DIA Oracle and integrate it into your projects. Data feeds are instantaneous and highly transparent. So, you can perform your transactions and deploy smart contracts based on them.

* Metis DIA Oracle Address: [0x6E6E633320Ca9f2c8a8722c5f4a993D9a093462E](https://andromeda-explorer.metis.io/address/0x6E6E633320Ca9f2c8a8722c5f4a993D9a093462E/transactions)
* DIA Oracle in Andromeda Explorer: [https://andromeda-explorer.metis.io/address/0x6E6E633320Ca9f2c8a8722c5f4a993D9a093462E/transactions](https://andromeda-explorer.metis.io/address/0x6E6E633320Ca9f2c8a8722c5f4a993D9a093462E/transactions)

### Get Started with the Idea <a href="#_fgunak5thj0h" id="_fgunak5thj0h"></a>

DIA Oracle allows you to receive helpful data pairs such as ETH/USD. We are going to create an application that gets instant ETH/USD price exchange using DIA. You can get several data pairs such as BTC, DIA, USDC, FTM, SDN, KSM, and MOVR as well.

Developers can use the explorer to see the details of how the contract was deployed, and the code and ABI key is also available.

Here is the verified source code of the contract that aims at getting price feed value ETH/USD:

_pragma solidity 0.7.4;_

_contract DIAOracle {_

_mapping (string => uint256) public values;_

_address oracleUpdater;_

_event OracleUpdate(string key, uint128 value, uint128 timestamp);_

_event UpdaterAddressChange(address newUpdater);_

_constructor() {_

_oracleUpdater = msg.sender;_

_}_

_function setValue(string memory key, uint128 value, uint128 timestamp) public {_

_require(msg.sender == oracleUpdater);_

_uint256 cValue = (((uint256)(value)) << 128) + timestamp;_

_values\[key] = cValue;_

_emit OracleUpdate(key, value, timestamp);_

_}_

_function getValue(string memory key) public view returns (uint128, uint128) {_

_uint256 cValue = values\[key];_

_uint128 timestamp = (uint128)(cValue % 2\*\*128);_

_uint128 value = (uint128)(cValue >> 128);_

_return (value, timestamp);_

_}_

_function updateOracleUpdaterAddress(address newOracleUpdaterAddress) public {_

_require(msg.sender == oracleUpdater);_

_oracleUpdater = newOracleUpdaterAddress;_

_emit UpdaterAddressChange(newOracleUpdaterAddress);_

_}_

_}_

The getValue() function allows you to get the value of a specific key according to the latest updated information. This key is a string, and each update emits an event containing the key of the updated value. The response value is an integer in a fixed comma format, and the timestamp associated with each value is a Unix timestamp for the UTC timezone.

Using the deployed source code, you can display the price of ETH in USD. So, let’s create a new directory and build our index.html and index.js files as follows:

$ mkdir pricefeed

$ cd pricefeed

Create the index.html and index.js files:

$ touch index.html

$ touch index.js

Note that the ABI key in your index.js file must be set according to the contract ABI key that can be copied from the [Andromeda Explorer](https://andromeda-explorer.metis.io/address/0x6E6E633320Ca9f2c8a8722c5f4a993D9a093462E/transactions). Also, the contract address must be changed to the Metis DIA address. Using a call in your code is mandatory to get the updated information.

const currentPriceResponse = await contract.methods.getValue('ETH/USD').call()

![](../../.gitbook/assets/0)

![](<../../.gitbook/assets/1 (5)>)

Index.js file should look like this:

async function connect() {

const priceElem = document.querySelector('#price')

try {

priceElem.textContent = 'Updating price...';

const abi = \[abi] // the contract’s ABI

const web3 = new Web3(new Web3.providers.HttpProvider("https://andromeda.metis.io/?owner=1088"))

const contract = new web3.eth.Contract(ABI, "0x6E6E633320Ca9f2c8a8722c5f4a993D9a093462E");

const currentPriceResponse = await contract.methods.getValue('ETH/USD').call()

if (currentPriceResponse\[0]) {

const formatter = new Intl.NumberFormat('en-US', {

style: 'currency',

currency: 'USD',

minimumFractionDigits: 2,

});

priceElem.textContent = formatter.format(currentPriceResponse\[0] / 100000000); // the output is in gwei so you would have to format to ether

}

} catch (e) {

priceElem.textContent = 'An error occurred while updating the price, please refresh your page.'; }

}

window.onload = function() {

connect();

}

You can style your index.html page as you like and put a div to get the ETH/USD exchange information.

Index.html:

\<h5>

\<span>Current Value: \<i id=”price”>\</i>\<span>

\</h5>

After setting up your index.html file and including index.js inside of it, you can simply run index.html to get the values.

Here is a [sample](https://elegant-heisenberg-02fd48.netlify.app/) of the price feed app deployed using Metis DIA oracle.

![](<../../.gitbook/assets/2 (11) (1)>)
