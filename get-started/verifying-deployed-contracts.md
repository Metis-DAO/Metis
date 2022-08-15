# Verifying Deployed Contracts on Metis

### hardhat-etherscan plugin

NOTE: make sure that your current `@nomiclabs/hardhat-etherscan` package version is [3.1.0](https://github.com/NomicFoundation/hardhat/releases/tag/%40nomiclabs%2Fhardhat-etherscan%403.1.0) or above.

If you haven't used this plugin, please read its [documentation](https://hardhat.org/plugins/nomiclabs-hardhat-etherscan.html) first.

Add Metis networks to your hardhat config file

```typescript
const config: HardhatUserConfig = {
  solidity: {
    version: "0.8.4",
    settings: {
      optimizer: {
        enabled: true,
        runs: 800,
      },
      // Don't use it for now
      // metadata: {
      //   bytecodeHash: "none",
      // },
    },
  },
  networks: {
    stardust: {
      url: "https://stardust.metis.io/?owner=588",
    },
    andromeda: {
      url: "https://andromeda.metis.io/?owner=1088",
    },
  },
  etherscan: {
    apiKey: {
      stardust: "a non-empty string or just use api-key",
      andromeda: "a non-empty string or just use api-key",
    },
    customChains: [
      {
        network: "andromeda",
        chainId: 1088,
        urls: {
          apiURL: "https://andromeda-explorer.metis.io",
          browserURL: "https://andromeda-explorer.metis.io",
        },
      },
      {
        network: "stardust",
        chainId: 588,
        urls: {
          apiURL: "https://stardust-explorer.metis.io",
          browserURL: "https://stardust-explorer.metis.io",
        },
      },
    ],
  },
};
```

Deploy your contracts

```
$ yarn hardhat run scripts/deploy.ts --network stardust
Generating typings for: 2 artifacts in dir: typechain for target: ethers-v5
Successfully generated 5 typings!
Compiled 2 Solidity files successfully
Greeter deployed to: 0x06F8CfB8d4d40ba65C64fC40F4A45218A1072eF5
```

Verify your contracts

```
$ yarn hardhat --network stardust verify  --contract contracts/Greeter.sol:Greeter 0x06F8CfB8d4d40ba65C64fC40F4A45218A1072eF5 'Hello, Hardhat!' 
Nothing to compile
No need to generate any newer typings.
Successfully submitted source code for contract
contracts/Greeter.sol:Greeter at 0x06F8CfB8d4d40ba65C64fC40F4A45218A1072eF5
for verification on the block explorer. Waiting for verification result...

Successfully verified contract Greeter on Etherscan.
https://stardust-explorer.metis.io/address/0x06F8CfB8d4d40ba65C64fC40F4A45218A1072eF5#code
✨  Done in 84.48s.
```

We have made a pull request for the changes, and it will be merged into upstream hardhat repository

### hardhat-deploy plugin

If you're using hardhat-deploy, it will be more easier! there is a hardhat config example

```ts
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
        runs: 800,
      },
      // There is a bug of explorer here
      // If you use this, you won't be able to verify your contract
      // metadata: {
      //   bytecodeHash: "none",
      // },
    },
  },
  networks: {
    stardust: {
      url: "https://stardust.metis.io/?owner=588",
      accounts: [process.env.PRIVATE_KEY],
      verify: {
        etherscan: {
          // just use api-key
          apiKey: "api-key",
          apiUrl: "https://stardust-explorer.metis.io",
        },
      },
    },
    andromeda: {
      url: "https://andromeda.metis.io/?owner=1088",
      accounts: [process.env.PRIVATE_KEY],
      verify: {
        etherscan: {
          // just use api-key
          apiKey: "api-key",
          apiUrl: "https://andromeda-explorer.metis.io",
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

Now you can run `yarn hardhat --network stardust etherscan-verify` to verify your contract!

Learn more about hardhat-deploy, please check out its [GitHub repository](https://github.com/wighawag/hardhat-deploy#4-hardhat-etherscan-verify)
