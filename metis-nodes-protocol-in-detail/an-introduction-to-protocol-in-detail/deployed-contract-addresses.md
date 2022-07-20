# Deployed Contract Addresses

The Metis protocol consists of several contracts that are deployed on some specific addresses. The contracts have been deployed on L2, L1, Andromeda, and Stardust testnet. You can use the etherscan explorer to see full details about deployed contract addresses. [The Metis Github page](https://github.com/MetisProtocol/mvm/tree/develop/packages/contracts/deployments) shows all the deployed contract addresses.

### LAYER 2

#### Chain IDs: <a href="#_20bez9ia20tw" id="_20bez9ia20tw"></a>

●     Mainnet: 10

●     Kovan: 69

●     Goerli: 420 _The contracts relevant for the majority of developers are OVM\_ETH and the cross-domain messengers. The L2 addresses don't change._

#### Predeploy contracts: <a href="#_jv2pqcvyiq3s" id="_jv2pqcvyiq3s"></a>



| Contract                 | Address                                    |
| ------------------------ | ------------------------------------------ |
| OVM\_L2ToL1MessagePasser | 0x4200000000000000000000000000000000000000 |
| OVM\_DeployerWhitelist   | 0x4200000000000000000000000000000000000002 |
| MVM\_ChainConfig         | 0x4200000000000000000000000000000000000005 |
| L2CrossDomainMessenger   | 0x4200000000000000000000000000000000000007 |
| OVM\_GasPriceOracle      | 0x420000000000000000000000000000000000000F |
| L2StandardBridge         | 0x4200000000000000000000000000000000000010 |
| OVM\_SequencerFeeVault   | 0x4200000000000000000000000000000000000011 |
| L2StandardTokenFactory   | 0x4200000000000000000000000000000000000012 |
| OVM\_L1BlockNumber       | 0x4200000000000000000000000000000000000013 |
| MVM\_Coinbase            | 0xDeadDeAddeAddEAddeadDEaDDEAdDeaDDeAD0000 |
| OVM\_ETH                 | 0x420000000000000000000000000000000000000A |

### LAYER 1

### TRIAL <a href="#_5wd20948j8ge" id="_5wd20948j8ge"></a>

#### Network : Rinkeby (chain id: 4)



| Contract                                         | Address                                                                                                                       |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| BondManager                                      | [0xF1C14733B0D7F2B08d2FeC14F93DA99662DbD217](https://rinkeby.etherscan.io/address/0xF1C14733B0D7F2B08d2FeC14F93DA99662DbD217) |
| CanonicalTransactionChain                        | [0x5c562387117599241Da6761A3039a11AC192983c](https://rinkeby.etherscan.io/address/0x5c562387117599241Da6761A3039a11AC192983c) |
| ChainStorageContainer-CTC-batches                | [0xC579b942D0B9d56cff8089f0AD7c10279C0C538c](https://rinkeby.etherscan.io/address/0xC579b942D0B9d56cff8089f0AD7c10279C0C538c) |
| ChainStorageContainer-CTC-queue                  | [0xf5EBC3F5465B155D8f25EdCF9BCfDB93a4Fab11c](https://rinkeby.etherscan.io/address/0xf5EBC3F5465B155D8f25EdCF9BCfDB93a4Fab11c) |
| ChainStorageContainer-SCC-batches                | [0x02b2a06aBD053ecEAFFde1B8685537fa83dbc7AA](https://rinkeby.etherscan.io/address/0x02b2a06aBD053ecEAFFde1B8685537fa83dbc7AA) |
| L1StandardBridge\_for\_verification\_only        | [0xca6D4e6030ceCc3d600a3E9fe6CC1D8370864696](https://rinkeby.etherscan.io/address/0xca6D4e6030ceCc3d600a3E9fe6CC1D8370864696) |
| Lib\_AddressManager                              | [0xD5D7825A091607CBD99531C409b16bE16aeFc7eA](https://rinkeby.etherscan.io/address/0xD5D7825A091607CBD99531C409b16bE16aeFc7eA) |
| MVM\_DiscountOracle                              | [0x96517e9CEae97c0f264ed83f35D4166C7B66a0B5](https://rinkeby.etherscan.io/address/0x96517e9CEae97c0f264ed83f35D4166C7B66a0B5) |
| MVM\_L2ChainManagerOnL1\_for\_verification\_only | [0xa90C255aFAd5dF326Ac59130A6A729048C025B5D](https://rinkeby.etherscan.io/address/0xa90C255aFAd5dF326Ac59130A6A729048C025B5D) |
| OVM\_L1CrossDomainMessenger                      | [0x69636924A27AF5A15ca6b10fBae5962e4958b9BF](https://rinkeby.etherscan.io/address/0x69636924A27AF5A15ca6b10fBae5962e4958b9BF) |
| Proxy\_\_MVM\_ChainManager                       | [0x372fcdBc28e6AFa27018Cda8eA698Ed83354EC5A](https://rinkeby.etherscan.io/address/0x372fcdBc28e6AFa27018Cda8eA698Ed83354EC5A) |
| Proxy\_\_OVM\_L1CrossDomainMessenger             | [0x18F9138FCed74163A9007c357EA91Df140071a36](https://rinkeby.etherscan.io/address/0x18F9138FCed74163A9007c357EA91Df140071a36) |
| Proxy\_\_OVM\_L1StandardBridge                   | [0xc293264DED30f60068eE394A74Ed3c038F650697](https://rinkeby.etherscan.io/address/0xc293264DED30f60068eE394A74Ed3c038F650697) |
| StateCommitmentChain                             | [0xF8703F8369E41a6734b28ED6Ed7608343176E0b6](https://rinkeby.etherscan.io/address/0xF8703F8369E41a6734b28ED6Ed7608343176E0b6) |

### STARDUST

#### Network : Rinkeby (chain id: 4) <a href="#_oim70cw7bicr" id="_oim70cw7bicr"></a>



| Contract                                           | Address                                                                                                                       |
| -------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| BondManager                                        | [0x9D9cb79c7741adD5A468FEaA7d8c9F21A9D16873](https://rinkeby.etherscan.io/address/0x9D9cb79c7741adD5A468FEaA7d8c9F21A9D16873) |
| CanonicalTransactionChain                          | [0x8872d61135E71745Da6Ddda1F98d4b79E599E889](https://rinkeby.etherscan.io/address/0x8872d61135E71745Da6Ddda1F98d4b79E599E889) |
| ChainStorageContainer-CTC-batches                  | [0xa04060eFAFE3c63De460E53151c0206A886576a0](https://rinkeby.etherscan.io/address/0xa04060eFAFE3c63De460E53151c0206A886576a0) |
| ChainStorageContainer-CTC-queue                    | [0x3f33339857C795a50E7F741C3df4C2abb9d97383](https://rinkeby.etherscan.io/address/0x3f33339857C795a50E7F741C3df4C2abb9d97383) |
| ChainStorageContainer-SCC-batches                  | [0x4e9F8D9CDE0f19490b7e6Cc04CE20F9612262C72](https://rinkeby.etherscan.io/address/0x4e9F8D9CDE0f19490b7e6Cc04CE20F9612262C72) |
| L1StandardBridge\_for\_verification\_only          | [0x7AE95D1241d7B27312baA8245dfAC80B08E2e68a](https://rinkeby.etherscan.io/address/0x7AE95D1241d7B27312baA8245dfAC80B08E2e68a) |
| Lib\_AddressManager                                | [0xC9EB2B0bD7dbA69bb72886E9cF5da34d1Ca88C38](https://rinkeby.etherscan.io/address/0xC9EB2B0bD7dbA69bb72886E9cF5da34d1Ca88C38) |
| MVM\_CanonicalTransaction                          | [0xCCB4a3279310Ed85A3ff1Ef84DE1a9d91fAF56e0](https://rinkeby.etherscan.io/address/0xCCB4a3279310Ed85A3ff1Ef84DE1a9d91fAF56e0) |
| MVM\_CanonicalTransaction\_for\_verification\_only | [0x76d4Fc1CB6D554ff9A065914A22C46df0ffB8A6D](https://rinkeby.etherscan.io/address/0x76d4Fc1CB6D554ff9A065914A22C46df0ffB8A6D) |
| MVM\_DiscountOracle                                | [0x9db3BedF13fa81a887DA2010470E4A5E49523239](https://rinkeby.etherscan.io/address/0x9db3BedF13fa81a887DA2010470E4A5E49523239) |
| MVM\_L2ChainManagerOnL1\_for\_verification\_only   | [0x23b1BFb369667cc0bDa7B1da628268d3531d1D38](https://rinkeby.etherscan.io/address/0x23b1BFb369667cc0bDa7B1da628268d3531d1D38) |
| MVM\_Verifier                                      | [0xA9b8E3a95e0E22352747Ab5395Ec535Cd113016a](https://rinkeby.etherscan.io/address/0xA9b8E3a95e0E22352747Ab5395Ec535Cd113016a) |
| MVM\_Verifier\_for\_verification\_only             | [0xe47bc1F78BFF44b144b4830f0651908012d1E99d](https://rinkeby.etherscan.io/address/0xe47bc1F78BFF44b144b4830f0651908012d1E99d) |
| OVM\_L1CrossDomainMessenger                        | [0xFbB32A0b32FE568B5e11829C83c4f20397c6f740](https://rinkeby.etherscan.io/address/0xFbB32A0b32FE568B5e11829C83c4f20397c6f740) |
| Proxy\_\_MVM\_CanonicalTransaction                 | [0x4fB8A54377d5c2D24a61Fb51D78cceC0B3221412](https://rinkeby.etherscan.io/address/0x4fB8A54377d5c2D24a61Fb51D78cceC0B3221412) |
| Proxy\_\_MVM\_ChainManager                         | [0x5553c94Cf01e1e631F9F92F26Afb1383F17a8D30](https://rinkeby.etherscan.io/address/0x5553c94Cf01e1e631F9F92F26Afb1383F17a8D30) |
| Proxy\_\_MVM\_Verifier                             | [0x33f81D2E1E1203A3186BE79022CC36C5b929E9f9](https://rinkeby.etherscan.io/address/0x33f81D2E1E1203A3186BE79022CC36C5b929E9f9) |
| Proxy\_\_OVM\_L1CrossDomainMessenger               | [0xfD1b91066D27345023eBE2FE0D4C59d78c46129f](https://rinkeby.etherscan.io/address/0xfD1b91066D27345023eBE2FE0D4C59d78c46129f) |
| Proxy\_\_OVM\_L1StandardBridge                     | [0x056999aea33e5A6e51b5cF24a0684d565dF741EF](https://rinkeby.etherscan.io/address/0x056999aea33e5A6e51b5cF24a0684d565dF741EF) |
| StateCommitmentChain                               | [0xA9917d31D30048Dcf257639FE777F6606A100F89](https://rinkeby.etherscan.io/address/0xA9917d31D30048Dcf257639FE777F6606A100F89) |

### ANDROMEDA

#### Network : mainnet (chain id: 1) <a href="#_r35zeomz0jv0" id="_r35zeomz0jv0"></a>



| Contract                                           | Address                                                                                                               |
| -------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| BondManager                                        | [0xf51B9C9a1c12e7E48BEC15DC358D0C1f0d7Eb3be](https://etherscan.io/address/0xf51B9C9a1c12e7E48BEC15DC358D0C1f0d7Eb3be) |
| CanonicalTransactionChain                          | [0x56a76bcC92361f6DF8D75476feD8843EdC70e1C9](https://etherscan.io/address/0x56a76bcC92361f6DF8D75476feD8843EdC70e1C9) |
| ChainStorageContainer-CTC-batches                  | [0x38473Feb3A6366757A249dB2cA4fBB2C663416B7](https://etherscan.io/address/0x38473Feb3A6366757A249dB2cA4fBB2C663416B7) |
| ChainStorageContainer-CTC-queue                    | [0xA91Ea6F5d1EDA8e6686639d6C88b309cF35D2E57](https://etherscan.io/address/0xA91Ea6F5d1EDA8e6686639d6C88b309cF35D2E57) |
| ChainStorageContainer-SCC-batches                  | [0x10739F09f6e62689c0aA8A1878816de9e166d6f9](https://etherscan.io/address/0x10739F09f6e62689c0aA8A1878816de9e166d6f9) |
| L1StandardBridge\_for\_verification\_only          | [0x101500214981e7A5Ad2334D8404eaF365C2c3113](https://etherscan.io/address/0x101500214981e7A5Ad2334D8404eaF365C2c3113) |
| Lib\_AddressManager                                | [0x918778e825747a892b17C66fe7D24C618262867d](https://etherscan.io/address/0x918778e825747a892b17C66fe7D24C618262867d) |
| MVM\_CanonicalTransaction\_for\_verification\_only | [0x431e877E216714647a4DCcEFFC03d7B4Fd4B825E](https://etherscan.io/address/0x431e877E216714647a4DCcEFFC03d7B4Fd4B825E) |
| MVM\_DiscountOracle                                | [0xC8953ca384b4AdC8B1b11B030Afe2F05471664b0](https://etherscan.io/address/0xC8953ca384b4AdC8B1b11B030Afe2F05471664b0) |
| MVM\_L2ChainManagerOnL1\_for\_verification\_only   | [0x9E2E3be85df5Ca63DE7674BA64ffD564075f3B48](https://etherscan.io/address/0x9E2E3be85df5Ca63DE7674BA64ffD564075f3B48) |
| MVM\_Verifier                                      | [0x9Ed4739afd706122591E75F215208ecF522C0Fd3](https://etherscan.io/address/0x9Ed4739afd706122591E75F215208ecF522C0Fd3) |
| MVM\_Verifier\_for\_verification\_only             | [0xB2e2060A179e67cA4299Cc79fA337B98791DE069](https://etherscan.io/address/0xB2e2060A179e67cA4299Cc79fA337B98791DE069) |
| OVM\_L1CrossDomainMessenger                        | [0x8bF439ef7167023F009E24b21719Ca5f768Ecb36](https://etherscan.io/address/0x8bF439ef7167023F009E24b21719Ca5f768Ecb36) |
| Proxy\_\_MVM\_CanonicalTransaction                 | [0x6A1DB7d799FBA381F2a518cA859ED30cB8E1d41a](https://etherscan.io/address/0x6A1DB7d799FBA381F2a518cA859ED30cB8E1d41a) |
| Proxy\_\_MVM\_ChainManager                         | [0xf3d58D1794f2634d6649a978f2dc093898FEEBc0](https://etherscan.io/address/0xf3d58D1794f2634d6649a978f2dc093898FEEBc0) |
| Proxy\_\_MVM\_Verifier                             | [0xe70DD4dE81D282B3fa92A6700FEE8339d2d9b5cb](https://etherscan.io/address/0xe70DD4dE81D282B3fa92A6700FEE8339d2d9b5cb) |
| Proxy\_\_OVM\_L1CrossDomainMessenger               | [0x081D1101855bD523bA69A9794e0217F0DB6323ff](https://etherscan.io/address/0x081D1101855bD523bA69A9794e0217F0DB6323ff) |
| Proxy\_\_OVM\_L1StandardBridge                     | [0x3980c9ed79d2c191A89E02Fa3529C60eD6e9c04b](https://etherscan.io/address/0x3980c9ed79d2c191A89E02Fa3529C60eD6e9c04b) |
| StateCommitmentChain                               | [0xf209815E595Cdf3ed0aAF9665b1772e608AB9380](https://etherscan.io/address/0xf209815E595Cdf3ed0aAF9665b1772e608AB9380) |

