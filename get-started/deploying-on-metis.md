# Deploying on Metis

{% hint style="info" %}
In case when you need higher limits of the public RPC endpoint, there are two options:\
1\. Setting up a private RPC endpoint using a provider such as [Nodies](https://www.nodies.app/) or [BlastAPI](https://blastapi.io/)\
2\. Setting up a [replica node](https://github.com/ericlee42/metis-replica-node)&#x20;
{% endhint %}

### Using Solidity Language and Remix IDE <a href="#jrhnn8um2jms" id="jrhnn8um2jms"></a>

Solidity is a free programming language you can use for deploying smart contracts. The Remix IDE helps us easily deploy our projects in a web-based programming environment. Before using the Remix IDE, you need to take a few steps and configure your MetaMask account. The MetaMask wallet is used as a tool to provide an account address and see the results.

* [_Configuring MetaMask and getting started with your account_](../readme/metamask-setting-how-to-connect-your-metamask-crypto-wallet-to-metis-platform.md)

Make sure you’re on the Metis Testnet in MetaMask. Otherwise, you can simply switch to the Metis HoleskyTestnet using the MetaMask networks section.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

You would have some Metis test tokens in your account if you got some using the instructions given in this section. Otherwise, you can follow the [instructions](https://docs.metis.io/dev/get-started/getting-test-tokens) to get Metis test tokens and bridge them from L1 to the Metis Testnet.

### What is OpenZeppelin? <a href="#imk2ztgyg14m" id="imk2ztgyg14m"></a>

OpenZeppelin is an open-source and powerful platform designed to offer simplicity and secure smart contract deployment. When dealing with Ethereum smart contract deployment, you need to write your code quickly and automatically. OpenZeppelin platform provides everything for you to create and automate Web 3 applications. Please refer to the [OpenZeppelin setup guide](https://docs.openzeppelin.com/contracts/4.x/) to configure everything if you haven’t done the configuration process. You can easily install the OpenZeppelin library with a terminal command.

### Method 1: How to Use OpenZeppelin to Prepare Smart Contracts? <a href="#vtc4nrfulhnh" id="vtc4nrfulhnh"></a>

#### Step 1 <a href="#id-7ds63un1508g" id="id-7ds63un1508g"></a>

Go to the [OpenZeppelin Contracts Wizard](https://wizard.openzeppelin.com/) page to start the first project. You will have an interactive generator that offers simplicity to generating your desired code.

Choose the settings you want to be applied and use a name and symbol for your contract.

You can even choose between ERC20 and ERC721 and check/uncheck several features to get your desired output. We set the test token to be mintable and burnable.

![](<../.gitbook/assets/2 (7) (1) (1)>)

#### Step 2 <a href="#id-5bbl0c7z5kmo" id="id-5bbl0c7z5kmo"></a>

After doing the settings and preparing your code, you have two options. You can copy the code to use on any deployment platform, or you have the option to click on “Open in Remix” to proceed to the Remix development environment.

![](<../.gitbook/assets/3 (6) (1)>)

#### Step 3 <a href="#le0z0pzal7wm" id="le0z0pzal7wm"></a>

You can select the Solidity compiler from the left to compile the smart contract. Use Auto-compile to perform everything with peace of mind, and after doing your desired settings, you can click on the “Compile contract” button.

Once you click the compile button, it works on your code and shows a message indicating that the compilation process was successful. Note that you can select the Solidity compiler version and EVM version. We check the optimization option to have cheaper transactions.

![](<../.gitbook/assets/4 (6) (1)>)

#### Step 4 <a href="#id-3gdz555hg8bb" id="id-3gdz555hg8bb"></a>

Working with the Remix IDE is really simple. You can do the compilation process in the blink of an eye and proceed to the next step. Select the “Deploy and run transactions” option from the left, and you can start deploying your smart contract using a wide range of possibilities here.

We will deploy our smart contract on a testnet using a MetaMask account. So, make sure your MetaMask account is configured and ready to use.

First, select the environment you want to deploy your smart contract. Choose the “Injected Provider” option from the environment menu. A pop-up window will appear, and you will be able to connect the Remix IDE to your desired MetaMask account.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

Once you have connected to Remix IDE, you can see that your account address has been successfully added to the Remix IDE.

#### Step 5 <a href="#id-3bpnlw326dw7" id="id-3bpnlw326dw7"></a>

After doing all the settings, you can click on the “Deploy” button. Be careful your MetaMask network must be switched to the Metis Testnet, and you must have enough Metis tokens in your account.

Click on the “Deploy” button, and you will see a MetaMask pop-up window. Confirm the deployment process to finish everything.

#### Step 6 <a href="#pbslhwfq4ja4" id="pbslhwfq4ja4"></a>

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

If everything goes well, MetaMask shows you a success message. You can then verify your smart contract deployment.

Click on the recent activity to see the details. You can check all the details and verify your transaction. So, click on the “View on block explorer” option to open a window with detailed information about your transaction deployment.\\

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

### Method 2: Configuring Hardhat to Get a Comprehensive Environment for Deploying Contracts <a href="#ytwknjghhuz" id="ytwknjghhuz"></a>

Hardhat is a highly reliable deployment environment that provides a secure and simple platform to deploy smart contracts. Hardhat allows developers to run Solidity code locally, and you can get a bunch of features in an integrated platform. Hardhat needs the latest version of Nodejs, and you can install it using different options. Use the instructions given on [the Node Js website](https://nodejs.org/en/) to install a supported version and then you can [install Hardhat](https://hardhat.org/getting-started) and configure the environment for smart contract deployment.

Head over to [https://hardhat.org/getting-started](https://hardhat.org/getting-started) and follow the instructions to install Hardhat on your system. You can easily install it on Windows, Linux, or Mac. Note that you need to install the latest version of Nodejs and Typescript on your system to run Hardhat commands without any error. More importantly, you need the npm package manager installed on your system.

![](<../.gitbook/assets/11 (10) (1)>)

Here are the required steps for configuring Hardhat, creating a project, and compiling it to be deployed on the Metis platform. We configure Hardhat for Linux (Debian-based distros), but the procedure is the same for Windows and Mac.

#### Step 1 <a href="#y8dckqmm9gpj" id="y8dckqmm9gpj"></a>

After installing the latest version of Git, Nodejs, and Hardhat, create a project folder for your sample project.

Use the following commands to create a demo project and initialize git for the project. You can create a folder and then change the working directory.

```bash
$ mkdir metis-demo
$ cd metis-demo
$ git init
$ npm init -y
```

![](<../.gitbook/assets/12 (2)>)

#### Step 2 <a href="#rac3kq7w7pwo" id="rac3kq7w7pwo"></a>

If it’s a new project and you have not run Hardhat in the project folder, running Hardhat for the first time gives you the following options. Select the 3rd option to create an advanced sample project that uses Typescript. You then need to hit the enter button to initialize the project. If everything goes well, you get a message showing that creating the project was successful.

`$ npx hardhat`

![](<../.gitbook/assets/13 (6) (1)>)

![](<../.gitbook/assets/14 (1) (1) (1)>)

![](<../.gitbook/assets/15 (5) (1)>)

#### Step 3 <a href="#nbstplwcrx9c" id="nbstplwcrx9c"></a>

First, you need to open the project folder using VSCode. Then edit the project config file and add the following lines of code to it.

```
metis: {
url: "https://sepolia.rpc.metisdevops.link/",
accounts:
process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : [],
},
```

#### Step 4 <a href="#id-6bnnsb85cieo" id="id-6bnnsb85cieo"></a>

You need to add your account private key to the .env file in your project. Follow these steps to get your account private key.

* Open your MetaMask and click on account details. You will be able to export your account private key and copy it to use for deploying your smart contract.

![](<../.gitbook/assets/19 (6) (1)>)

#### Step 5 <a href="#lpcfgk9c9lld" id="lpcfgk9c9lld"></a>

We use a test code to deploy our first smart contract. So, let's leave it unchanged and compile it to start deploying the first smart contract.

![](<../.gitbook/assets/20 (7) (1)>)

#### Step 6 <a href="#id-8algqnqj1fcz" id="id-8algqnqj1fcz"></a>

Use the following command to compile the smart contract. Then, you can test by using the test command. If everything is good, you can get the success message.

```
$ npx hardhat compile
$ npx hardhat test
```

![](<../.gitbook/assets/image (48).png>)

![](<../.gitbook/assets/image (31) (2).png>)

#### Step 7 <a href="#isqlwn3wt0pd" id="isqlwn3wt0pd"></a>

You can now deploy your smart contract using the following command.

`$ npx hardhat run scripts/deploy.ts --network metis`

![](<../.gitbook/assets/image (7) (2).png>)

#### Step 8 <a href="#id-662vdat5z0oc" id="id-662vdat5z0oc"></a>

Let’s check the results on the Metis platform. Click on the “View account explorer” option to open the Metis Testnet explorer website.

You can explore your last transactions here and check that your last smart contract deployment was successful.

You can explore your last transactions here and check that your last smart contract deployment was successful.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

Please feel free to reach out to our [Help Center](https://metisdao.atlassian.net/servicedesk/customer/portals) if you have any technical questions.
