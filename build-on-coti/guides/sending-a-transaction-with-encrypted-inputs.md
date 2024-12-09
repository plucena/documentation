# Sending a Transaction with Encrypted Inputs

{% hint style="info" %}
This guide assumes you have already deployed the Counter.sol contract. If you haven't already, check out our guide for deploying a [Basic Private Smart Contract](basic-private-smart-contract.md).
{% endhint %}

Now that we have acquired our AES encryption key, we can use it to encrypt the arguments for calling the `add` function of our `Counter.sol` contract.

{% tabs %}
{% tab title="TypeScript (Server)" %}
#### Setup

```bash
npm install @coti-io/coti-ethers
```

#### Code

<pre class="language-typescript"><code class="lang-typescript">import { Contract, CotiNetwork, getDefaultProvider, Wallet } from "@coti-io/coti-ethers"

<strong>const PRIVATE_KEY = "&#x3C;EOA_PRIVATE_KEY>"
</strong>const AES_KEY = "&#x3C;AES_KEY>"
const COUNTER_ADDRESS = "&#x3C;COUNTER_ADDRESS>"
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

const itValue = await wallet.encryptValue(
    123n,
    COUNTER_ADDRESS,
    counter.add.fragment.selector
)

await (
    await counter.add(itValue)
).wait()
</code></pre>
{% endtab %}

{% tab title="TypeScript (Browser)" %}
#### Setup

```bash
npm install @coti-io/coti-ethers
```

#### Code

```typescript
import { BrowserProvider, Contract, Eip1193Provider, JsonRpcSigner } from '@coti-io/coti-ethers'

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

const itValue = await signer.encryptValue(
    BigInt(123),
    COUNTER_ADDRESS,
    counter.add.fragment.selector
)

await (
    await counter.add(itValue)
).wait()
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

it_value = account.encrypt_value(
    123,
    counter_contract.address,
    counter_contract.functions.add(value=(0, bytes(65))).selector
)

tx = counter_contract.functions.add(it_value).build_transaction({
    'from': account.address,
    'chainId': w3.eth.chain_id,
    'nonce': w3.eth.get_transaction_count(account.address),
    'gas': 15000000,
    'gasPrice': w3.to_wei(30, 'gwei')
})

signed_tx = w3.eth.account.sign_transaction(tx, PRIVATE_KEY)

tx_hash = w3.eth.send_raw_transaction(signed_tx.raw_transaction)

w3.eth.wait_for_transaction_receipt(tx_hash)
```
{% endtab %}
{% endtabs %}
