# Resolving a Transaction's Encrypted Outputs

{% hint style="info" %}
This guide assumes you have already deployed the Counter.sol contract. If you haven't already, check out our guide for deploying a [Basic Private Smart Contract](basic-private-smart-contract.md).
{% endhint %}

After calling `add`, we can use our AES encryption key to decrypt the value of `sum` and check the result.

{% tabs %}
{% tab title="TypeScript (Server)" %}
#### Setup

```bash
npm install @coti-io/coti-ethers
```

#### Code

```typescript
import { Contract, CotiNetwork, getDefaultProvider, Wallet } from "@coti-io/coti-ethers"

const PRIVATE_KEY = "<EOA_PRIVATE_KEY>"
const AES_KEY = "<AES_KEY>"
const COUNTER_ADDRESS = "<COUNTER_ADDRESS>"
const COUNTER_ABI = [
    {
        "inputs": [],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
    {
        "inputs": [
            {
                "components": [
                    {
                        "internalType": "ctUint64",
                        "name": "ciphertext",
                        "type": "uint256"
                    },
                    {
                        "internalType": "bytes",
                        "name": "signature",
                        "type": "bytes"
                    }
                ],
                "internalType": "struct itUint64",
                "name": "value",
                "type": "tuple"
            }
        ],
        "name": "add",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "sum",
        "outputs": [
            {
                "internalType": "ctUint64",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]

const provider = getDefaultProvider(CotiNetwork.Testnet)
const wallet = new Wallet(PRIVATE_KEY, provider)
wallet.setAesKey(AES_KEY)

const counter = new Contract(COUNTER_ADDRESS, COUNTER_ABI, wallet)

const ctSum = await counter.sum()

const clearSum = await wallet.decryptValue(ctSum)

console.log(clearSum)
```
{% endtab %}

{% tab title="TypeScript (Browser)" %}
#### Setup

```bash
npm install @coti-io/coti-ethers
```

#### Code

```typescript
import { Contract, CotiNetwork, getDefaultProvider, Wallet } from "@coti-io/coti-ethers"

const PRIVATE_KEY = "<EOA_PRIVATE_KEY>"
const AES_KEY = "<AES_KEY>"
const COUNTER_ADDRESS = "<COUNTER_ADDRESS>"
const COUNTER_ABI = [
    {
        "inputs": [],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
    {
        "inputs": [
            {
                "components": [
                    {
                        "internalType": "ctUint64",
                        "name": "ciphertext",
                        "type": "uint256"
                    },
                    {
                        "internalType": "bytes",
                        "name": "signature",
                        "type": "bytes"
                    }
                ],
                "internalType": "struct itUint64",
                "name": "value",
                "type": "tuple"
            }
        ],
        "name": "add",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "sum",
        "outputs": [
            {
                "internalType": "ctUint64",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]

const provider = new BrowserProvider(window.ethereum as Eip1193Provider)
const signer = await provider.getSigner()
signer.setAesKey(AES_KEY)

const counter = new Contract(COUNTER_ADDRESS, COUNTER_ABI, signer)

const ctSum = await counter.sum()

const clearSum = await signer.decryptValue(ctSum)

console.log(clearSum)
```
{% endtab %}

{% tab title="Python" %}
#### Setup

```bash
pip install coti-web3
```

#### Code

```python
from eth_account import Account
from eth_account.signers.local import LocalAccount
from web3 import Web3
from web3.utils.coti import (
  CotiNetwork,
  init_web3
)

PRIVATE_KEY = "<EOA_PRIVATE_KEY>"
AES_KEY = "<AES_KEY>"
COUNTER_ADDRESS = "<COUNTER_ADDRESS>"
COUNTER_ABI = [
    {
        "inputs": [],
        "stateMutability": "nonpayable",
        "type": "constructor"
    },
    {
        "inputs": [
            {
                "components": [
                    {
                        "internalType": "ctUint64",
                        "name": "ciphertext",
                        "type": "uint256"
                    },
                    {
                        "internalType": "bytes",
                        "name": "signature",
                        "type": "bytes"
                    }
                ],
                "internalType": "struct itUint64",
                "name": "value",
                "type": "tuple"
            }
        ],
        "name": "add",
        "outputs": [],
        "stateMutability": "nonpayable",
        "type": "function"
    },
    {
        "inputs": [],
        "name": "sum",
        "outputs": [
            {
                "internalType": "ctUint64",
                "name": "",
                "type": "uint256"
            }
        ],
        "stateMutability": "view",
        "type": "function"
    }
]

account: LocalAccount = Account.from_key(PRIVATE_KEY, { 'aes_key': AES_KEY })
w3: Web3 = init_web3(CotiNetwork.TESTNET)

counter_contract = w3.eth.contract(address=COUNTER_ADDRESS, abi=COUNTER_ABI)

ct_sum = counter_contract.functions.sum().call({'from': account.address})

clear_sum = account.decrypt_value(ct_sum)

print(clear_sum)
```
{% endtab %}
{% endtabs %}
