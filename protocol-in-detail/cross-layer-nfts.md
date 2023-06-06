# Cross Layer NFTs

### What Are Cross-layer NFTs? <a href="#_4t9f1uo01vxl" id="_4t9f1uo01vxl"></a>

A significant problem on the Ethereum platform and Layer 2 is the segmentation of assets, especially NFTs. NFTs stick to the layer that they have been created on and the problem leads to NFT silos. According to this problem, NFTs that were created on L2 cannot be withdrawn to L1.

Blockchain experts have designed a novel approach to deal with that problem called the cross-layer NFT. The solution is very important as we need to use cross-layer NFTs for decentralized applications and Web 3.0 tools.

### Solution <a href="#_wu2qi5ujws7m" id="_wu2qi5ujws7m"></a>

The solution is to design a way for the transfer of NFTs across layers. Also, using this method, we can reduce the gas requirements by minting the NFTs on the blockchain network. This method lets users decide when they want to withdraw their desired NFTs to L1.

### Pros & Cons of the Solution <a href="#_qesuemdd3fw" id="_qesuemdd3fw"></a>

#### Pros <a href="#_tmvowwsk43cs" id="_tmvowwsk43cs"></a>

* Allows any blockchain user to claim an NFT on another layer.
* The user of layer 2 can withdraw an NFT and have an equivalent NFT created on layer 1 without duplication.
* The users have the option to transfer NFTs created on the blockchain ecosystem at any time from one layer to another.
* Project owners can extend their NFTs to multiple rollups.
* The method is significantly a low-cost solution and it provides speed and ease of use.

#### Cons <a href="#_vdyh6s52wdta" id="_vdyh6s52wdta"></a>

* The owner has to create an equivalent NFT contract on all supported layers.
* We have only the option to move NFTs from L2 to the coordinator chain (L1) and back. NFTs cannot be moved from layer 2 to layer 2 directly.
* There are higher costs for moving NFTs among rollups. It’s because of the involvement of two layer-1 transactions (withdrawal and deposit).
* There are some technical complexities to connecting different layers to move NFTs .

### Let’s Check the Diagram <a href="#_czixkmze9cs1" id="_czixkmze9cs1"></a>

Here is a representation of the solution, helping you get the idea. Making use of this method, users can have their NFT contracts on both blockchain layers and withdraw/deposit at any time they want.

![](<../.gitbook/assets/0 (1)>)

### Initializing the NFT contract <a href="#_7jl4io2sjucn" id="_7jl4io2sjucn"></a>

Stage 1 shows that the owner should provision the `L1_NFT_Collection` with the deployed `L2_NFT_Collection`. In this case, the owner must specify the following information:

* Address of the `L2_NFT_Collection`
* Chain ID of the `L2_NFT_Collection`
* Location of the NFT, mapping of the Chain ID by the NFT ID
* The range of allocated NFTs, for example, 1-100, 60-61, etc.

The data structure is as follows:

`mapping(chainid=>address) addresses`;

`mapping(rangeid => chainid) range_loc`;

The NFT contracts and deposit contracts must be created before proceeding. Note that the range is a unit of 1000 NFTs. The rangeid 0 means NFT #0 to NFT #999.

### Claiming the NFT on Layer 2 <a href="#_l4j3l0dblag3" id="_l4j3l0dblag3"></a>

According to stage 2, users can claim the NFT by specifying the NFT ID. The L2\_NFT\_Collection contract is used to verify the ID assignment and mint the NFT accordingly.

Users can transfer NFTs using this process within the rollup that makes the idea of cross-platform NFTs real.

### Withdrawing an NFT <a href="#_siw01k92qe84" id="_siw01k92qe84"></a>

Cross-platform NFT withdrawal is performed in a 4-level process as stated below:

* Stage 3: Users can send a transaction request to the `L2_NFT_Collection` contract. This process will initiate the NFT withdrawal to a target recipient on Layer 1.
* Stage 4: `L2_Deposit` contract is used to keep the deposited NFT. The NFT is locked here for later retrieval if necessary in the future.
* Stage 5: The locked NFT triggers a cross-message to the `L1_NFT_Collection` contract. This message allows the designated recipient to mint or withdraw the NFT on layer 1 if needed in the future. Keep the following points in mind in terms of stage 5:
  * A new record in the `L1_NFT_Collection` contract will be added to update the chain ownership of the particular NFT. The data structure is `mapping(id => chainid) nft_loc;` which `nft_loc` always overrides `range_loc`.
  * In order to determine the actual chain ownership of an NFT, the logic should always check `nft_loc` first and then `range_loc` in the case `nft_loc[id]` returns 0.
  * If the NFT was previously minted on layer 1, the user can get it with the updated metadata and its updated `nft_loc`. This situation occurs when an NFT has been withdrawn to layer 1 before.
* Stage 6: The recipient can claim the NFT by giving the NFT ID.

### (Re)Depositing NFT Process on Layer 2 <a href="#_bkb2vzasmfxu" id="_bkb2vzasmfxu"></a>

The depositing or redepositing process on layer 2 consists of 3 main stages. The process starts with sending a transaction to the `L1_NFT_collection` contract and ends with claiming or minting the NFT on L1:

* Stage 7: Users can send a transaction to the `L1_NFT_Collection` smart contract. This transaction initiates the transition to a target recipient on a target layer 2 rollup platform.
* Stage 8: The NFT gets deposited into the `L1_Deposit` contract and the NFT is locked for a later retrieval process.
* Stage 9: The locked NFT triggers a cross-message to the `L2_NFT_Collection` contract. So, the message allows users to mint or claim the NFT on layer 2 by giving the NFT ID. In this stage, note some points:
  * If the NFT was already created and deposited to the target rollup, users can get it with the updated metadata.
  * The `L1_NFT_Collection` contract has `nft_loc` updated to reflect and respect the updated chain ownership of the NFT.

### Sequence Diagrams <a href="#_b005l11dq749" id="_b005l11dq749"></a>

The following sequence diagrams give you full insight into cross-platform NFTs and the process of claiming, withdrawing, and depositing cross-platform NFTs.

#### Provisioning <a href="#_et5xhwky07eq" id="_et5xhwky07eq"></a>

![](<../.gitbook/assets/1 (1)>)

### Layer 1 => Layer 2 <a href="#_42yg4sfauifw" id="_42yg4sfauifw"></a>

![](../.gitbook/assets/2)

### Layer 2 => Layer 1 <a href="#_jxdvm7fwhiwn" id="_jxdvm7fwhiwn"></a>

![](<../.gitbook/assets/3 (2) (2)>)

### Extensions <a href="#_2lsm4c7e9k9" id="_2lsm4c7e9k9"></a>

We can reduce transactions’ costs using a rollup by tracking chain ownership. A rollup platform can be used in this case to manage and explore the chain’s ownership. Using this solution, a transaction’s cost on the blockchain layer can be significantly reduced. The method can be applied to the L1 blockchain and it helps us reduce costs like other rollups in some ways.

Please feel free to reach out to our [Help Center](https://metisdao.atlassian.net/servicedesk/customer/portals) if you have any technical questions.
