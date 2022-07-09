# What is Witnet\_

### What is Witnet? <a href="#_3kwo0ui623c3" id="_3kwo0ui623c3"></a>

[Witnet](https://witnet.io/) is a high-power crypto oracle that enables your projects to reach out off-chain data feeds. Witnet is providing smart contracts with a simple and straightforward method to use online and trusted data sources.

You can integrate Witnet data feeds into your smart contract and provide users with online information about external sources. You can use the [Witnet docs](https://docs.witnet.io/) to see more details about the supported platforms and read how-to-use instructions.

### Metis Witnet Oracle <a href="#_pgezbachtdpv" id="_pgezbachtdpv"></a>

[Metis is partnered with Witnet](https://docs.witnet.io/smart-contracts/supported-chains) and is providing price data feeds on the mainnet (Andromeda) and testnet (Stardust). There are Witnet Price Routers for the Andromeda and Stardust network that you can use to receive data pairs right in your smart contracts.

* Metis Mainnet (Andromeda): [0xD39D4d972C7E166856c4eb29E54D3548B4597F53](https://andromeda-explorer.metis.io/address/0xD39D4d972C7E166856c4eb29E54D3548B4597F53/read-contract)

| Caption            | ID4        | Heartbeat | Deviation | Cooldown |
| ------------------ | ---------- | --------- | --------- | -------- |
| Price-METIS/USDT-6 | 0x4ba45817 | 24 hours  | 2.0%      | 15'      |

* Metis Testnet (Stardust): [0x5134EAF08bcf8cE1922991150AAad1774e93751f](https://stardust-explorer.metis.io/address/0x5134EAF08bcf8cE1922991150AAad1774e93751f/transactions)

| Caption            | ID4        | Heartbeat | Deviation | Cooldown |
| ------------------ | ---------- | --------- | --------- | -------- |
| Price-BTC/USD-6    | 0x24beead4 | 24 hours  | 1.0%      | 15'      |
| Price-ETH/USD-6    | 0x3d15f701 | 24 hours  | 1.0%      | 15'      |
| Price-METIS/USDT-6 | 0x4ba45817 | 24 hours  | 1.0%      | 15'      |

### Get started with the Metis Witnet Oracle <a href="#_fjcj61hj56ze" id="_fjcj61hj56ze"></a>

Witnet oracle allows you to get data feed pairs for several blockchain tokens. We are going to provide you how to get METIS/USDT data feeds, but you can use this method to receive data for ETH/USD, BTC, USD, etc.

You need to use the [WitnetPriceRouter](https://andromeda-explorer.metis.io/address/0xD39D4d972C7E166856c4eb29E54D3548B4597F53/read-contract) in your code that was deployed and verified on the Andromeda mainnet.

Here is the [verified source code](https://andromeda-explorer.metis.io/address/0xD39D4d972C7E166856c4eb29E54D3548B4597F53/contracts) and ABI for the WitnetPriceRouter that provides you with different data feeds such as METIS/USDT.

You will need to copy the contract address and ABI key to use in your project. We create a new project with the index.html and index.js files.

$ mkdir Metis\ Witnet

$ cd Metis\ Witnet

$ touch index.html

$ touch index.js

You can style your index.html file as you like. The price tag is used to show the data feed received from the Witnet Price Router. Note that the valueFor() function is used to get the latest data feed and you can specify METIS/USD data feed using the following value.

| Price feed         | ID         |
| ------------------ | ---------- |
| Price-METIS/USDT-6 | 0x4ba45817 |

Put the following information in your index.js file to receive your desired data feeds.

* The http provider address: https://andromeda.metis.io/?owner=1088
* Get function: valueFor('0x4ba45817')
* Witnet Price Verifier Contract address = "0xD39D4d972C7E166856c4eb29E54D3548B4597F53"

Finally, your index.html and index.js files should look like this:

![](<../.gitbook/assets/0 (3)>)

![](<../.gitbook/assets/1 (6)>)

You can run the html file after configuring the network and addresses.

![](<../.gitbook/assets/2 (7)>)

Here is a [Github repository](https://github.com/Metisdvlpr/Metis-Witnet) for the application that you can download and test it yourself.
