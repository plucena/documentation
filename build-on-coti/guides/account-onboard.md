# Account Onboard

{% hint style="info" %}
This guide assumes you have already deployed the Counter.sol contract. If you haven't already, check out our guide for deploying a [Basic Private Smart Contract](basic-private-smart-contract.md).
{% endhint %}

In order to interact with the `Counter.sol` smart contract from the previous section, we are going to need to acquire our AES encryption key. To do so, we can use either COTI's [**Ethers**](https://app.gitbook.com/o/-MgoVlq5Hr-DSFn_cBMH/s/eC83qbrBhITO4kE7kTNB/~/changes/1/build-on-coti/tools/ethers) package or COTI's [web3.py](../tools/web3.py.md).

{% tabs %}
{% tab title="TypeScript (Server)" %}
#### Setup

```bash
npm install @coti-io/coti-ethers
```

#### Code

```typescript
import { CotiNetwork, getDefaultProvider, Wallet } from "@coti-io/coti-ethers"

const PRIVATE_KEY = "<EOA_PRIVATE_KEY>"

const provider = getDefaultProvider(CotiNetwork.Testnet)
const wallet = new Wallet(PRIVATE_KEY, provider)

await wallet.generateOrRecoverAes()

console.log(wallet.getUserOnboardInfo()?.aesKey)
```
{% endtab %}

{% tab title="TypeScript (Browser)" %}
Setup

```bash
npm install @coti-io/coti-ethers
```

#### Code

```typescript
import { BrowserProvider, Eip1193Provider, JsonRpcSigner } from '@coti-io/coti-ethers'

const provider = new BrowserProvider(window.ethereum as Eip1193Provider)
const signer = await provider.getSigner()
await signer.generateOrRecoverAes()

console.log(signer.getUserOnboardInfo()?.aesKey)
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
  generate_or_recover_aes,
  init_web3
)

PRIVATE_KEY = "<EOA_PRIVATE_KEY>"

account: LocalAccount = Account.from_key(PRIVATE_KEY)
w3: Web3 = init_web3(CotiNetwork.TESTNET)

generate_or_recover_aes(w3, account)

print(account.aes_key)
```
{% endtab %}
{% endtabs %}
