# Metis Audits

### Metis Audits <a href="#_z13hcss2oce1" id="_z13hcss2oce1"></a>

An audit is a type of examination that is performed to analyze coding, testing, and security issues. Audits are absolutely crucial in the case of smart contracts, and the blockchain network since blockchain security companies ensure users and organizations there are no errors and vulnerabilities.

The following sections include important reports provided by the leading blockchain security firms. You can simply download the final reports and prove Metis’s security and safety.

* [Download the Hacken report](https://drive.google.com/file/d/1AHDVzVUcRh8ghmfLR8qRfaHpgML7v9vW/view)
* [Download the SlowMist report](https://drive.google.com/file/d/1d6tjopE25mN50ByLacX8IpoIuWSGKwQ8/view)
* [Download the Armors Lab report](https://drive.google.com/file/d/1yv\_PBW-hanMoB50gan4zB2QKsEKNxyOO/view)

### Metislab Hacken.io <a href="#_90zmbxjr2src" id="_90zmbxjr2src"></a>

Hacken is a cybersecurity company that provides businesses with cybersecurity solutions. Hacken is delivering security analysis and code reviews for customers so businesses and their customers can know vulnerabilities and potential issues when introducing and using a service.

Hacken.io code review and security analysis for Metislab foundation were conducted from September 13th, 2021, to September 17th, 2021. Aimed at smart contracts and vulnerabilities, the scope of the Hacken project report is to give customers complete insight into the level of security for the Metis protocol. You can explore the Metis protocol in the following repository.

* Repository: [https://github.com/MetisProtocol/Metis-Mining](https://github.com/MetisProtocol/Metis-Mining)
* Commit: 0b83dd3290050a534f8883788a195e350cbc1e8f

The deployed platform is the Ethereum network, and the analyzed smart contract has been written in Solidity language. The methods for security analysis and review are as follows:

* Architecture review
* Functional testing
* Computer-aided verification
* Manual review

JS tests have been considered and here is the list of the contracts tested and analysed:

* common\Address.sol
* common\Context.sol
* common\EnumerableSet.sol
* common\ERC20.sol
* common\IERC20.sol
* common\Math.sol
* common\Ownable.sol
* common\ReentrancyGuard.sol
* common\SafeERC20.sol
* common\SafeMath.sol
* interfaces\IDAC.sol
* interfaces\IDACRecorder.sol
* interfaces\IDistributor.sol
* interfaces\IMetisToken.sol
* interfaces\IMining.sol
* interfaces\IVault.sol
* mock\MockDAC.sol
* mock\MockMetisToken.sol
* DACRecorder.sol
* Distributor.sol
* Mining.sol
* Vault.sol

### Highly Secure and Works Fine <a href="#_psxzvdpo27ed" id="_psxzvdpo27ed"></a>

The report you can review at the bottom of this page includes all check items that were performed during the security analytics. Hacken engineers have scanned the smart contract and its dependencies for commonly known security issues and more specific vulnerabilities.

#### According to the report, the Metislab smart contracts are well-secured and approved for security considerations. <a href="#_tys2sju847wg" id="_tys2sju847wg"></a>

After the second review conducted on September 23rd, 2021, Hacken security engineers found that all issues were fixed.

The items checked under the “code review” category include the following parts:

* Reentrancy
* Ownership takeover
* Timestamp dependence
* Gas limit and loops
* DoS with (unexpected) throw
* DoS with block gas limit
* Transaction-ordering dependence
* Style guide violation
* Costly loop
* ERC20 API violation
* Unchecked external call
* Unchecked maths
* Unsafe type inference
* Implicit visibility level
* Deployment consistency
* Repository consistency
* Data consistency

Also, the “functional review” category consists of the following items checked and analyzed:

* Business logics review
* Functionality checks
* Access control & authorization
* Escrow manipulation
* Token supply manipulation
* Assets integrity
* User balances manipulation
* Data consistency manipulation
* Kill-switch mechanism
* Operation trails & event generation

[Download the Hacken Report](https://drive.google.com/file/d/1AHDVzVUcRh8ghmfLR8qRfaHpgML7v9vW/view)

### SlowMist Metis <a href="#_55lakujliaj5" id="_55lakujliaj5"></a>

[SlowMist](https://www.slowmist.com/) is a third-party blockchain security company aiming to test and improve blockchain network solutions. It started in 2018 and has more than 10 years of approved experience in providing security services.

Metis team’s security audit application was submitted on October 18th, 2021. The application’s main goal was to analyze the project source code for security issues and vulnerabilities.

* Project source code: [https://github.com/MetisProtocol/mvm](https://github.com/MetisProtocol/mvm)
* Audit version: commit 5a470def3318088fd8e3a47aeda762c08cc6ff2e

There are 6 levels of smart contract vulnerabilities and issues used to scrutinize the Metis project and its protocols.

* Critical
* High risk
* Medium risk
* Low risk
* Weaknesses
* Enhancement suggestions

The main scope of the audit is as follows:

* Code Compliance Audit: Passed
* Random Number Generation Algorithm Audit: Passed
* Keystore Audit: Passed
* Cryptographic Component Call Audit: Passed
* Encryption Strength Audit: Passed
* Length Extension Attack Audit: Passed
* Transaction Malleability Attack Audit: Passed
* Replay Attack Audit: Passed
* Top-up Program Audit: Passed
* RPC Permission Audit: Passed
* Static Code Analysis: Passed

The overall result of the SlowMist audit showed no small, medium, or critical vulnerabilities. Also, all SlowMist suggestions and small fix needs have been done by Metis.

* [Download the SlowMist report](https://drive.google.com/file/d/1d6tjopE25mN50ByLacX8IpoIuWSGKwQ8/view)

### Armors Lab Metis <a href="#_v68q5yhrug1u" id="_v68q5yhrug1u"></a>

[Armors Lab](https://armors.io/) is a leading blockchain security firm focused on empowering smart contracts and the blockchain network. The company’s main purpose is to use the Haskell language to develop a security and monitoring framework. Armors Lab delivers smart contract monitoring services, real-time analysis, and security audits.

Metis portal audit was started on December 18th, 2021, and the security analysis was passed with 0 vulnerabilities in the 4 security levels as below:

* Critical severity
* High severity
* Medium severity
* Low severity

The main goal of the security test was to analyze the Metis protocol and smart contracts:

* Code URL: [https://github.com/MetisProtocol/Metis-Mining](https://github.com/MetisProtocol/Metis-Mining)
* Commit : a943fea02123441b4485650538ab21bc14eb2c70

Here is the summary of audit results provided by Armors Lab:

* Re-Entrancy: Safe
* Arithmetic Over/Under Flows: Safe
* Unexpected Blockchain Currency: Safe
* Delegatecall: Safe
* Default Visibilities: Safe
* Entropy Illusion: Safe
* External Contract Referencing: Safe
* Short Address/Parameter Attack: Safe
* Unchecked CALL Return Values: Safe
* Race Conditions/Front Running: Safe
* Denial Of Service (DOS): Safe
* Block Timestamp Manipulation: Safe
* Constructors with Care: Safe
* Uninitialised Storage Pointers: Safe
* Floating Points and Numerical Precision: Safe
* Tx.origin Authentication: Safe
* Permission restrictions: Safe

You can download the final report showing that the Metis protocol and smart contracts have passed the test with 0 issues.

* [Download the Armors Lab report](https://drive.google.com/file/d/1yv\_PBW-hanMoB50gan4zB2QKsEKNxyOO/view)
