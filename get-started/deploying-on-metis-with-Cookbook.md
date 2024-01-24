# Deploying on Metis using Cookbook.dev

[Cookbook.dev](https://wwww.cookbook.dev/?utm=metisdocs) is an open-source smart contract registry where developers can find solidity primitives, libraries, and smart contracts for protocols.

In this tutorial, we'll walk through searching for a protocol on Cookbook and deploying it to Metis using Cookbook's no-code deploy and using Cookbook with Remix, and Hardhat.

### Prerequisites

Before you begin, ensure you've:

- Set up your wallet
- Funded your wallet with METIS token on either the testnet or mainnet

## Search Cookbook's Smart Contract Registry

Navigate to [cookbook.dev/chains/Metis](https://www.cookbook.dev/chains/Metis?utm=metisdocs) and explore **Protocols** on Metis, or search for specific smart contracts in the search bar. 

![](<../.gitbook/assets/Metis_Cookbook_contract_search.png>)

To learn about a smart contract on Cookbook, select the protocol, and select `Expand`. This opens the code alongside ChefGPT, Cookbook's AI Solidity assistant. 

Highlight selections of the code and press **Analyze Snippet** to get more information about the smart contract code you're looking at, or ask ChefGPT questions about Metis, solidity, or your smart contract.

![](<../.gitbook/assets/Metis_Cookbook_ChefGPT.png>)

## Import any Smart Contract Code into Cookbook

Import verified smart contract code into Cookbook to fork, learn about, or build with by inputting any smart contract address into the Cookbook search bar.  

![](<../.gitbook/assets/Metis_Cookbook_import.png>)

## No-code Deploy your Smart Contract to Metis with Cookbook 

Choose **No-Code Deploy** on select (usually simpler) smart contracts on [Cookbook](https://www.cookbook.dev/contracts/simple-token?utm=metisdocs). We'll use a simple ERC20 token contract for this example. 

Connect your Wallet to Cookbook.dev. 

Set your smart contract arguments within the Cookbook UI (if applicable). 

![](<../.gitbook/assets/Metis_Cookbook_no_code_deploy_0.png>)

Select Metis or tMetis (Metis Testnet) under **Pick Chain**.

![](<../.gitbook/assets/Metis_Cookbook_no_code_deploy_1.png>)

Select **Deploy** and pay the network fee. 

![](<../.gitbook/assets/Metis_Cookbook_no_code_deploy_2.png>)

![](<../.gitbook/assets/Metis_Cookbook_no_code_deploy_3.png>)

Manage your deployed smart contract under **My Dashboard** in Cookbook.  

![](<../.gitbook/assets/Metis_Cookbook_no_code_deploy_3.png>)

Read or write to your newly deployed smart contract in the Cookbook Dashoard.

![](<../.gitbook/assets/Metis_Cookbook_no_code_deploy_4.png>)


## Deploy your Smart Contract to Metis with Remix

### Method #1 - Using the Cookbook.dev Website and Opening in Remix

On a smart contract or protocol page in Cookbook, select the **Open in Remix** option. Your smart contract will automatically be opened in a new Remix workspace.

**Compile** your smart contract within Remix. Most contracts opened with Cookbook will automatically compile within Remix. 

![](<../.gitbook/assets/Metis_Cookbook_Remix_compile.png>)

Once compiled, **deploy** the smart contract in Remix. 

Connect your wallet with Metis or Metis Sepolia testnet by selecting injected provider - Metamask Wallet in the **environments** tab within the **deploy** screen. 

![](<../.gitbook/assets/Metis_Cookbook_Remix_Deploy.png>)

Once deployed, we can interact with our smart contract within Remix.

### Method #2 - Using the Cookbook Remix Plug-in within the Remix IDE

Go to [Remix.Ethereum.org](https://remix.ethereum.org)

Add The Cookbook Plugin to Remix by clicking the Chef Hat Logo under **Featured Plugins** on the Remix Homepage.

Alternatively, search Cookbook and select **Activate** in the Remix Plugin Manager. 

Search for any protocol or smart contract and click the search result to import the smart contract code into Remix.

![](<../.gitbook/assets/Metis_Cookbook_Remix_plugin.png>)

Cookbook's AI solidity co-pilot, ChefGPT, is available within the Remix plugin to answer questions about Metis, Solidity, or the smart contract you're working with.

Compile and deploy the smart contract as described in **Method 1** above. 

## Deploy your Smart Contract to Metis with Hardhat

After finding the smart contract or protocol you want to work with in [Cookbook](https://www.cookbook.dev/?utm=metisdocs), select the **Download Source** option and select **Hardhat** to download the contract boilerplate. For this guide, we'll use [Cookbook's Simple ERC-20 Token Smart Contract](https://www.cookbook.dev/contracts/simple-token?utm=metisdocs).

To install the required packages and dependencies, run

```
npm install
```

To compile your smart contract, run 

```
npx hardhat compile
``` 

Add arguments to the `constructorArgs` array in the `deploy.js` file in the `scripts` folder and save.  If you do not need any arguments please leave the array empty.

In your `.env.example` file, add your wallet private key for the wallet containing Metis for network fees. Afterward change the name of the file to .env and create a gitignore to ignore your .env file.

In the `hardhat.config.js` file, uncomment out the example hardat network code
```
 metisAndromeda: {
      url: "https://andromeda.metis.io/?owner=1088",
      accounts: [process.env.WALLET_PRIVATE_KEY],
      verify: {
        etherscan: {
          apiKey: "apiKey is not required, just set a placeholder",
          apiUrl:
            "https://api.routescan.io/v2/network/mainnet/evm/1088/etherscan",
        },
      },
    },
```

To deploy your smart contract to Metis, run

```
npx hardhat run --network metisAndromeda scripts/deploy.js
``` 

Hardhat will return the deployed smart contract address in your terminal. View and verify your smart contract on the [Metis Block Explorer](https://andromeda-explorer.metis.io/).

**Further guidance**

For more information on using Cookbook to find, learn about or build with smart contracts, check out the following resources:

- [Documentation](https://docs.cookbook.dev/)
- [Blog](https://medium.com/@cookbookdev)
- [Twitter](https://twitter.com/cookbook_dev)
- [Community](https://discord.gg/cookbook)