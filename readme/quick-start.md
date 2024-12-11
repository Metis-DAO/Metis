# Quick start

<details>

<summary>Update for Shanghai EVM features</summary>

**1. PUSH0 opcode**

You can use shanghai evm version in your project.

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.20;
contract MetisShanghaiUpgradeExample {
    uint256 public num;
    function push0(uint256 _n) public {
        num = _n;
    }
}
```

**2. More user-friendly error from the rpc**

The RPC can return encoded custom errors and solidity panic errors

**Custom errors**

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.20;
error MyError();
contract MetisShanghaiUpgradeExample {
    function customError() public pure {
        revert MyError();
    }
}
```

{% code overflow="wrap" %}
```sh
curl https://l2rpc.devnet.metisdevops.link -X POST -H "Content-Type: application/json" \
  --data '{"method":"eth_call","params":[{"to":"0x2e07967571dB8896178A65039b4Dd13Be354B002","input":"0xdda3a7bd"}, "latest"],"id":1,"jsonrpc":"2.0"}'
```
{% endcode %}

**The error data 0xdd6c951c is for the custom error MyError**

{% code overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": { "code": 3, "message": "execution reverted", "data": "0xdd6c951c" }
}
```
{% endcode %}

**Solidity panic errors**

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.20;
contract MetisShanghaiUpgradeExample {
    function panicError(uint256 _x) public pure returns (uint256){
        return _x / 0;
    }
}
```

{% code overflow="wrap" %}
```sh
curl https://l2rpc.devnet.metisdevops.link -X POST -H "Content-Type: application/json" \
  --data '{"method":"eth_call","params":[{"to":"0x2e07967571dB8896178A65039b4Dd13Be354B002","input":"0x67c9d7930000000000000000000000000000000000000000000000000000000000000001"}, "latest"],"id":1,"jsonrpc":"2.0"}'
```
{% endcode %}

{% code overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": 3,
    "message": "execution reverted: division or modulo by zero",
    "data": "0x4e487b710000000000000000000000000000000000000000000000000000000000000012"
  }
}
```
{% endcode %}

</details>

<table><thead><tr><th width="197">Basic info</th><th width="284">Mainnet</th><th>Testnet</th></tr></thead><tbody><tr><td>Add network to your wallet</td><td><a href="https://chainlist.org/chain/1088">Andromeda</a></td><td> <a href="https://chainlist.org/chain/59902">Sepolia</a></td></tr><tr><td>Solidity version</td><td>0.8.26</td><td>Same as mainnet</td></tr><tr><td>RPC</td><td>https://andromeda.metis.io/?owner=1088</td><td>https://sepolia.metisdevops.link</td></tr><tr><td>Fork of EVM</td><td>Berlin</td><td>Same as mainnet</td></tr><tr><td>Unsupported EIPs</td><td><p>EIPs from Shanghai update are not supported for now (check the notice at the top)</p><p>Also 1559, 4337 arent supported.</p></td><td>Same as mainnet</td></tr><tr><td>Unsupported infra</td><td><ul><li>Tenderly (use paid <a href="https://www.alchemy.com/">Alchemy</a>)</li><li>The Graph (use <a href="https://www.ormilabs.xyz/">Ormigraph</a> or paid <a href="https://www.alchemy.com/">Alchemy</a>)</li></ul></td><td>Same as mainnet</td></tr><tr><td>Gas fee formula</td><td>50 / METIS token price (in gwei)</td><td>Same as mainnet</td></tr><tr><td>Gas limit</td><td>1.1bn</td><td>Same as mainnet</td></tr><tr><td>Faucet for test tokens</td><td>Nope</td><td><a href="https://faucet-427702.uc.r.appspot.com/">Link</a></td></tr></tbody></table>

## Canonical contract addresses of tokens

<table><thead><tr><th width="114">Token</th><th width="541">Address</th><th>Decimals</th></tr></thead><tbody><tr><td>METIS</td><td>0xDeadDeAddeAddEAddeadDEaDDEAdDeaDDeAD0000</td><td>18</td></tr><tr><td>m.USDC</td><td>0xEA32A96608495e54156Ae48931A7c20f0dcc1a21</td><td>6</td></tr><tr><td>m.USDT</td><td>0xbB06DCA3AE6887fAbF931640f67cab3e3a16F4dC</td><td>6</td></tr><tr><td>wETH</td><td>0x420000000000000000000000000000000000000A</td><td>18</td></tr><tr><td>wMETIS</td><td>0x75cb093E4D61d2A2e65D8e0BBb01DE8d89b53481</td><td>18</td></tr><tr><td>m.wBTC</td><td>0x433E43047B95cB83517abd7c9978Bdf7005E9938</td><td>8</td></tr></tbody></table>
