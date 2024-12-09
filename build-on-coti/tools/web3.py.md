# Web3.py

In order to provide easy access to all the features of COTI, the [**coti-web3**](https://github.com/coti-io/coti-web3.py) Python SDK was created, which is made in a way that has an interface very similar to that of [**web3.py**](https://web3py.readthedocs.io/en/stable/). In fact, web3 is a peer dependency of our library and all of the objects exported by coti-web3 (e.g. LocalAccount, Web3, etc.) inherit from the corresponding web3 objects and extend their functionality where needed.

While most of the existing SDKs should work out of the box, using unique COTI features like encrypting transaction inputs, requires executing the onboarding procedure and encrypting using the defined protocol.

The library is made in such a way that after replacing web3 with `coti-web3` most client apps will work out of box.

## Clone, Build & Test

Full instructions for building and testing the package locally are available in the [coti-web3.py GitHub repository](https://github.com/coti-io/coti-web3.py)

## Installation

```bash
pip install coti-web3
```

{% hint style="info" %}
SDK versions < 1.0.0 are for use with the Devnet, versions >= 1.0.0 are for use with the Testnet
{% endhint %}

## Usage

See the [**coti-python-examples**](https://github.com/coti-io/coti-python-examples) repository for code examples.

## Classes

### Account

<pre class="language-python"><code class="lang-python"><strong>def from_key(private_key: PrivateKeyType, user_onboard_info: UserOnboardInfo = None)
</strong></code></pre>

* **Description**: Loads an account from a raw private key.
* **Parameters**:
  * `private_key`: The private key as a hex string, `bytes`, `int`, or `PrivateKey` object.
  * `user_onboard_info` (optional): An dictionary containing the information from the user's onboarding procedure
* **Returns**: A `LocalAccount` instance.

```python
def from_mnemonic(mnemonic: str, passphrase: str = "", account_path: str = ETHEREUM_DEFAULT_PATH, user_onboard_info: UserOnboardInfo = None)
```

* **Description**: Derives an account from a BIP39 mnemonic phrase.
* **Parameters**:
  * `mnemonic`: The mnemonic phrase.
  * `passphrase` (optional): A passphrase for added security.
  * `account_path` (optional): Custom HD path.
  * `user_onboard_info` (optional): A dictionary containing the information from the user's onboarding procedure
* **Returns**: A `LocalAccount` instance.

```python
def encrypt_value(plaintext_value: Union[bool, int, str], contract_address: str, function_selector: str, private_key: PrivateKeyType, aes_key: str)
```

* **Description**: Encrypts a plaintext value for secure contract inputs.
* **Parameters**:
  * `plaintext_value`: A boolean, unsigned integer or string to be encrypted
  * `contract_address`: The address of the contract that the user is interacting with
  * `function_selector`: The four-byte function selector
  * `private_key`: The private key used to sign the transaction and encrypted value
  * `aes_key`: The AES key used to encrypt the value
* **Returns**: Encrypted data object (`ItBool`, `ItUint`, or `ItString`)

```python
def decrypt_value(ciphertext: Union[CtBool, CtUint, CtString], aes_key: str)
```

* **Description**: Decrypts a value encrypted with `encrypt_value`.
* **Parameters**:
  * `ciphertext`: The encrypted ciphertext to decrypt
  * `aes_key`: The AES key used to decrypt the value
* **Returns**: The original value (`bool`, `int`, or `str`).

### LocalAccount

#### Properties

* `user_onboard_info`: A dictionary containing the information from the user's onboarding procedure
* `aes_key`: The AES key used for encryption and decryption
* `rsa_key_pair`: A tuple containing the RSA public and private keys
* `onboard_tx_hash`: The transaction hash associated with the onboarding procedure

#### Methods

```python
def __init__(key: PrivateKey, account: AccountLocalActions, user_onboard_info: UserOnboardInfo = None)
```

* **Description**: Initializes a new `LocalAccount` instance with a private key and an associated `AccountLocalActions` instance.
* **Parameters**:
  * `key` (PrivateKey): The private key for the account.
  * `account` (AccountLocalActions): Key management and signing API.
  * `user_onboard_info` (optional): Metadata associated with the account.

```python
def set_user_onboard_info(user_onboard_info: UserOnboardInfo)
```

* **Description**: Updates the onboarding metadata.
* **Parameters**:
  * `user_onboard_info`: Metadata dictionary containing user-specific information.

```python
def set_aes_key(aes_key: str)
```

* **Description**: Sets the AES key in the onboarding metadata.
* **Parameters**:
  * `aes_key`: The AES key.

```python
def set_rsa_key_pair(rsa_key_pair: tuple[str, str])
```

* **Description**: Sets the RSA key pair in the onboarding metadata.
* **Parameters**:
  * `rsa_key_pair`: A tuple of RSA public and private keys.

```python
def set_onboard_tx_hash(tx_hash: str)
```

* **Description**: Sets the onboarding transaction hash in the metadata.
* **Parameters**:
  * `tx_hash`: The transaction hash.

```python
def encrypt_value(plaintext_value: Union[bool, int, str], contract_address: str, function_selector: str)
```

* **Description**: Encrypts a plaintext value for secure smart contract interactions.
* **Parameters**:
  * `plaintext_value`: The value to encrypt (`bool`, `int`, or `str`).
  * `contract_address`: The contract's Ethereum address.
  * `function_selector`: The function selector for the contract call.
* **Returns**: An encrypted value (`ItBool`, `ItUint`, or `ItString`).

```python
def decrypt_value(ciphertext: Union[CtBool, CtUint, CtString])
```

* **Description**: Decrypts an encrypted value.
* **Parameters**:
  * `ciphertext`: The encrypted value (`CtBool`, `CtUint`, or `CtString`).
* **Returns**: The decrypted value (`bool`, `int`, or `str`).

## Modules

### COTI

```python
def init_web3(coti_network: CotiNetwork)
```

* **Description**: Initializes a Web3 client for the specified COTI network.
* **Parameters**:
  * `coti_network` (`CotiNetwork`): Network configuration (e.g., testnet or mainnet).
* **Returns**: A `Web3` instance configured with the appropriate middleware.

```python
def onboard(web3: Web3, account: LocalAccount, onboard_contract_address: str)
```

* **Description**: Onboards an Ethereum account by generating RSA keys, signing the public key, and interacting with the onboarding smart contract. It also retrieves and stores the AES key.
* **Parameters**:
  * `web3` (`Web3`): A Web3 instance.
  * `account` (`LocalAccount`): The Ethereum account to onboard.
  * `onboard_contract_address` (`str`): Address of the onboarding contract.
* **Returns**: None. Updates the `LocalAccount` instance with onboarded information.
* **Raises**:
  * `RuntimeError` if the account has insufficient funds for the transaction.

```python
def recover_aes_from_tx(web3: Web3, account: LocalAccount, onboard_contract_address: str)
```

* **Description**: Recovers the AES key for an account using transaction receipt data from the onboarding process.
* **Parameters**:
  * `web3` (`Web3`): A Web3 instance.
  * `account` (`LocalAccount`): The Ethereum account with onboarding metadata.
  * `onboard_contract_address` (`str`): Address of the onboarding contract.
* **Returns**: None. Updates the `LocalAccount` instance with the recovered AES key.

```python
def generate_or_recover_aes(web3: Web3, account: LocalAccount, onboard_contract_address: str = ACCOUNT_ONBOARD_CONTRACT_ADDRESS)
```

* **Description**: Ensures the account has an AES key by either recovering it from the onboarding transaction or initiating the onboarding process.
* **Parameters**:
  * `web3` (`Web3`): A Web3 instance.
  * `account` (`LocalAccount`): The Ethereum account.
  * `onboard_contract_address` (`str`): Address of the onboarding contract (optional, defaults to `ACCOUNT_ONBOARD_CONTRACT_ADDRESS`).
* **Returns**: None. Updates the `LocalAccount` with AES key information.
* **Raises**:
  * `RuntimeError` if the account has insufficient funds.
