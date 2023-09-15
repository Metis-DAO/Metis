# Verifying Deployed Contracts

NOTE: `hardhat-verify`,successor to `hardhat-etherscan`, if you still use legacy `hardhat-etherscan` package, make sure that your current package version is [3.1.0](https://github.com/NomicFoundation/hardhat/releases/tag/%40nomiclabs%2Fhardhat-etherscan%403.1.0) or above.

If you haven't used this plugin, please read its [documentation](https://hardhat.org/hardhat-runner/plugins/nomicfoundation-hardhat-verify) first.

Add Metis networks to your hardhat config file, **there is no api key, you can use a non-empty string**.

```solidity
const config: HardhatUserConfig = {
  solidity: {
    version: "0.8.4",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
      evmVersion: "berlin",
      metadata: {
        bytecodeHash: "none",
      },
    },
  },
  networks: {
    metisgoerli: {
      url: "https://goerli.gateway.metisdevops.link",
    },
    andromeda: {
      url: "https://andromeda.metis.io/?owner=1088",
    },
  },
  etherscan: {
    apiKey: {
      metisgoerli: "apiKey is not required, just set a placeholder",
      andromeda: "apiKey is not required, just set a placeholder",
    },
    customChains: [
      {
        network: "andromeda",
        chainId: 1088,
        urls: {
          apiURL: "https://api.routescan.io/v2/network/mainnet/evm/1088/etherscan",
          browserURL: "https://explorer.metis.io",
        },
      },
      {
        network: "metisgoerli",
        chainId: 599,
        urls: {
          apiURL: "https://goerli.explorer.metisdevops.link/api",
          browserURL: "https://goerli.explorer.metisdevops.link",
        },
      },
    ],
  },
};
```

### Deploy your contracts

```
$ yarn hardhat run scripts/deploy.ts --network metisgoerli
Generating typings for: 2 artifacts in dir: typechain for target: ethers-v5
Successfully generated 5 typings!
Compiled 2 Solidity files successfully
Greeter deployed to: 0x06F8CfB8d4d40ba65C64fC40F4A45218A1072eF5
```

### Verify your contracts

```
$ yarn hardhat --network metisgoerli verify  --contract contracts/Greeter.sol:Greeter 0x06F8CfB8d4d40ba65C64fC40F4A45218A1072eF5 'Hello, Hardhat!' 
Nothing to compile
No need to generate any newer typings.
Successfully submitted source code for contract
contracts/Greeter.sol:Greeter at 0x06F8CfB8d4d40ba65C64fC40F4A45218A1072eF5
for verification on the block explorer. Waiting for verification result...

Successfully verified contract Greeter on Etherscan.
https://goelri.explorer.metisdevops.link/address/0x06F8CfB8d4d40ba65C64fC40F4A45218A1072eF5#code
✨  Done in 84.48s.
```

## hardhat-deploy plugin

There is a hardhat config example for hardhat-deploy plugin

```solidity
import * as dotenv from "dotenv";

import { HardhatUserConfig } from "hardhat/config";
import "@nomiclabs/hardhat-waffle";
import "@typechain/hardhat";
import "hardhat-gas-reporter";
import "solidity-coverage";
import "hardhat-deploy";
import "./ts-src/scripts/accounts";

dotenv.config();

if (!process.env.PRIVATE_KEY) {
  throw new Error("No private key");
}

const config: HardhatUserConfig = {
  solidity: {
    version: "0.8.4",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
      evmVersion: "berlin",
      metadata: {
        bytecodeHash: "none",
      },
    },
  },
  networks: {
    metisgoerli: {
      url: "https://goerli.gateway.metisdevops.link",
      accounts: [process.env.PRIVATE_KEY],
      verify: {
        etherscan: {
          apiKey: "apiKey is not required, just set a placeholder",
          apiUrl: "https://goerli.explorer.metisdevops.link",
        },
      },
    },
    andromeda: {
      url: "https://andromeda.metis.io/?owner=1088",
      accounts: [process.env.PRIVATE_KEY],
      verify: {
        etherscan: {
          apiKey: "apiKey is not required, just set a placeholder",
          apiUrl: "https://api.routescan.io/v2/network/mainnet/evm/1088/etherscan",
        },
      },
    },
  },
  namedAccounts: {
    deployer: 0,
  },
  gasReporter: {
    enabled: process.env.REPORT_GAS !== undefined,
    currency: "USD",
  },
  paths: {
    tests: "ts-src/test",
    deploy: "ts-src/deploy",
  },
};

export default config;
```

Now you can run `yarn hardhat --network metisgoerli etherscan-verify` to verify your contract!

Learn more about hardhat-deploy, please check out its [GitHub](https://github.com/wighawag/hardhat-deploy#4-hardhat-etherscan-verify) repository
