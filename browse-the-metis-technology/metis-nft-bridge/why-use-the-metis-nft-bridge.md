# Metis NFT Bridge

### Why Use the Metis NFT Bridge? <a href="#_aidba0h9lz5v" id="_aidba0h9lz5v"></a>

An NFT bridge is a helping hand used to connect different networks throughout the blockchain ecosystem. Most of the time, you need to send NFTs from one test network to another on the Ethereum platform. NFT bridging is a helpful solution when you want to send an Ethereum test token from Rinkeby (testnet) to another test network (Goerli).

Note that every network (Rinkeby, Goerli, Kovan, etc) has its own protocols. So, an NFT bridge solution must meet their requirements to establish a trustworthy connection between them.

We need NFT bridging in order to reach the following benefits:

* Facilitating the transfer of NFTs between different networks on Ethereum
* Making NFTs accessible everywhere
* Accessing all the features of different networks

### How Does an NFT Bridge Work? <a href="#_49mxqee6ni5t" id="_49mxqee6ni5t"></a>

The working mechanism of an NFT bridge is simple and painless. Technically speaking, the user should deposit the NFT into the smart contract of network A. In this case, we want to bridge between network A and network B.

So, the NFT gets locked. The user needs to get signatures from the oracles of the network A indicating the deposit process was performed correctly. Using these signatures, users can call the same contract in the network B and claim their NFT.

### What is NFT Wrapping? <a href="#_oa8dgnvyd8x1" id="_oa8dgnvyd8x1"></a>

Wrapping is an important term in the blockchain network. Wrapping refers to exchanging a set of standards for another set of standards. Our goal is to provide error-free interaction between two networks using wrapped NFTs.

Blockchain networks such as Ethereum or Bitcoin have their exclusive protocols. So, we need to adapt standards of different networks to each other in order to make the bridging idea real. Wrapped tokens allow developers to use them on any network, and transfer value between blockchains. Because of the safe algorithms used in NFT wrapping, it’s safe, reliable, and trusted.

* Allows non-native tokens to be used on any blockchain platforms
* Allows us to perform bridging
* Allows developers to decrease transaction speed and fees

### Which Bridging Platforms Are Available Now? <a href="#_uhf8buzhzgyk" id="_uhf8buzhzgyk"></a>

Metis NFT Bridge offers a simple and quick method for wrapping and bridging NFTs between Ethereum networks. You can use Metis Bridge to send tokens from L1 to L2 and it provides you with several features, including low gas fees, speed of transactions, and safe operations.

With the help of Metis Bridge, developers can wrap tokens and bridge between L1 Ethereum (mainnet) and L2 Andromeda (Metis mainnet). Also, there is an option to bridge between the Ethereum testnet (Rinkeby) and the Metis testnet (Goerli).

### How to Wrap NFTs? <a href="#_cw7nquxep38c" id="_cw7nquxep38c"></a>

According to the network protocol, all NFTs must be wrapped before using the bridge method. A wrapped NFT makes it available to easily and safely bridge between L1 to L2.

The Metis NFT Bridge platform supports ERC#721 and ERC#1155 token standards. So, you need to follow the next steps in order to wrap your desired NFTs and bridge them using this feature.

The following steps can be applied for both L1 to L2 or L2 to L1.

#### Step 1 <a href="#_7p8iurr8cjyf" id="_7p8iurr8cjyf"></a>

Enter the [official bridge website](https://nftbridge.metis.io/bridge) of Metis and you can see the different options available for bridging NFTs. Once you have entered the website, you can connect your wallet with MetaMask. Click on the “Connect Wallet” button to proceed. Note that you need to switch to Rinkeby testnet in your MetaMask account and there is a need for enough ETH to pay for gas fees. If you don’t have any, read the instructions to get enough ETH and METIS test tokens.

#### ![](<../../.gitbook/assets/0 (2)>) <a href="#_9q6djuqocqej" id="_9q6djuqocqej"></a>

#### Step 2 <a href="#_vigujvx6rxw3" id="_vigujvx6rxw3"></a>

By clicking on the “Connect Wallet” option, a MetaMask window appears and you can select your account to connect to the Metis Bridge platform.

![](<../../.gitbook/assets/1 (1) (2)>)

![](<../../.gitbook/assets/2 (6)>)

#### Step 3 <a href="#_na8l7z4mhkgx" id="_na8l7z4mhkgx"></a>

After connecting your wallet to the Metis NFT Bridge, you can enter the NFT token address. Click on “I’m a developer, take me to deploy wrapped NFT” and enter your NFT address.

We have created an NFT with the name of MyToken on the Goerli testnet. So, The Metis system will automatically process the details and then you need to confirm to proceed.

![](<../../.gitbook/assets/3 (11)>)

![](<../../.gitbook/assets/4 (17)>)

#### Step 4 <a href="#_h8gy6pf6mp06" id="_h8gy6pf6mp06"></a>

Let the platform interact with your MetaMask account and confirm the gas fee to continue the process.

![](<../../.gitbook/assets/5 (1)>)

![](<../../.gitbook/assets/6 (9)>)

#### Step 5 <a href="#_n9jhwncz8at7" id="_n9jhwncz8at7"></a>

So, you have your wrapped NFT token address and you now need to click on the “Take me there” button. By clicking on the option, a new window will be shown that requires some information about your NFT token and its wrapped NFT address.

![](<../../.gitbook/assets/7 (13)>)

#### Step 6 <a href="#_kkdity156atd" id="_kkdity156atd"></a>

Users can submit a new issue and Metis will process your requests in a few days and provide you with the information when it's ready to bridge. Provide your wrapped NFT address copied from the previous step. Also, you need to get the contract address from the Goerli explorer and the transaction hash as well.

![](<../../.gitbook/assets/8 (2)>)

![](../../.gitbook/assets/9)

![](<../../.gitbook/assets/10 (8)>)

### An Important Point <a href="#_h85t2nrkmb1f" id="_h85t2nrkmb1f"></a>

Metis NFT Bridge platform calls the following functions for ERC721 and ERC1155 tokens. So, you must grant the function permissions for the L2 bridge address as stated below.

* ERC721: mint(address \_to, uint256 \_tokenId)
* ERC1155: mint(address \_to, uint256 \_tokenId, bytes memory data) and mintBatch(address \_to, uint256\[] memory \_ids, uint256\[] memory \_amounts, bytes memory \_data)

The bridge addresses that you need to consider when granting the function permissions:

* Andromeda: 0x5EA23Cb3D609F4522a21ADcC9Ca366e76C23c40f
* Goerli: 0xCF7257A86A5dBba34bAbcd2680f209eb9a05b2d2

You can read more details about the ERC721 and ERC1155 token wrapping on the Metis Github.

* ERC721: https://github.com/MetisProtocol/nftbridge/blob/main/contracts/mock/ERC721Mock.sol#L31-L33
* ERC1155: https://github.com/MetisProtocol/nftbridge/blob/main/contracts/mock/ERC1155Mock.sol#L22-L28

### How to Bridge NFTs Between L1 and L2? <a href="#_nq9v8sgcfrzx" id="_nq9v8sgcfrzx"></a>

Users and developers can use the Metis NFT Bridge solution to transfer tokens from L1 to L2 and from L2 to L1. Here are the required steps to do the process for L2 to L1, but the same procedure applies to the L1 to L2 blockchain.

#### Step 1 <a href="#_cfz1x1gjk0t4" id="_cfz1x1gjk0t4"></a>

Once your wrapped NFT token is approved, you can open the Metis NFT Bridge website and perform the process in a few seconds. Fill out the following form and you need to confirm the gas fee and allow the website to connect to your MetaMask account.

![](<../../.gitbook/assets/11 (5)>)

#### Step 2 <a href="#_foftgqoi8y6p" id="_foftgqoi8y6p"></a>

By clicking on the “Approve” button, you need to confirm the transfer and allow the website to connect to your MetaMask.

![](<../../.gitbook/assets/12 (3)>)

![](<../../.gitbook/assets/13 (14)>)

#### Step 4 <a href="#_bi9j27qlg83d" id="_bi9j27qlg83d"></a>

Note that the transfer from L2 to L1 blockchain takes about one week to be performed. You can click on “Confirm I will wait for 8 days” and the process starts to be confirmed.

![](<../../.gitbook/assets/14 (11)>)

![](<../../.gitbook/assets/15 (12)>)

### How to Check Transaction History? <a href="#_d7viwzo3imlo" id="_d7viwzo3imlo"></a>

Searching the transaction history is a very simple job using Metis NFT Bridge. Open [the link](https://nftbridge.metis.io/bridge) and scroll down to see “View history”. You can click on it and check the transactions and full details in one place. You can also use the [Etherscan explorer](https://etherscan.io/) to see the latest transactions and history of an NFT token all on one platform.

![](<../../.gitbook/assets/16 (3)>)

![](<../../.gitbook/assets/17 (2)>)

### &#x20;<a href="#_hflu41th36ar" id="_hflu41th36ar"></a>
