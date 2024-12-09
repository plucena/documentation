# Python SDK

The [**COTI Python SDK**](https://github.com/coti-io/coti-sdk-python) provides a set of encryption, decryption, and cryptographic utilities, including RSA and AES encryption, message signing, and key handling functions. The utilities are primarily designed to work with cryptographic operations for secure communication and message signing, particularly within Ethereum smart contracts or similar environments.

## Clone, Build & Test

Full instructions for building and testing the package locally are available in the [coti-sdk-python GitHub repository](https://github.com/coti-io/coti-sdk-python)

## Installation

```bash
pip install coti-sdk
```

{% hint style="info" %}
SDK versions < 1.0.0 are for use with the Devnet, versions >= 1.0.0 are for use with the Testnet
{% endhint %}

## Usage

See the [**coti-sdk-python-examples**](https://github.com/coti-io/coti-sdk-python-examples) repository for code examples.

## Modules

### Crypto

```python
def encrypt(key, plaintext)
```

Encrypts a 128-bit `plaintext` using the provided 128-bit AES `key` and ECB mode.

* **Parameters:**
  * `key (bytes)`: 128-bit AES key (16 bytes).
  * `plaintext (bytes)`: 128-bit or smaller plaintext to encrypt.
* **Returns:**
  * `ciphertext (bytes)`: The encrypted text.
  * `r (bytes)`: A random value used during encryption.

```python
def decrypt(key, r, ciphertext)
```

Decrypts the `ciphertext` using the provided 128-bit AES `key` and the random value `r`.

* **Parameters:**
  * `key (bytes)`: 128-bit AES key.
  * `r (bytes)`: Random value used during encryption.
  * `ciphertext (bytes)`: Encrypted text to decrypt.
* **Returns:**
  * `plaintext (bytes)`: The decrypted original message.

```python
def generate_aes_key()
```

Generates a random 128-bit AES key.

* **Returns:**
  * `key (bytes)`: Randomly generated 128-bit key (16 bytes).

```python
def sign_input_text(sender, addr, function_selector, ct, key)
```

Signs an input message composed of multiple parts, including sender address, contract address, function selector, and ciphertext.

* **Parameters:**
  * `sender (bytes)`: Sender address.
  * `addr (bytes)`: Contract address.
  * `function_selector (str)`: Ethereum function selector (in hex).
  * `ct (bytes)`: Ciphertext (concatenated).
  * `key (bytes)`: Private key for signing.
* **Returns:**
  * `signature (bytes)`: Digital signature of the message.

```python
def sign(message, key)
```

Signs a `message` using the provided `key` (Ethereum-style private key).

* **Parameters:**
  * `message (bytes)`: Message to sign.
  * `key (bytes)`: Private key to use for signing.
* **Returns:**
  * `signature (bytes)`: The generated signature.

```python
def build_input_text(plaintext, user_aes_key, sender, contract, function_selector, signing_key)
```

Encrypts a plaintext integer and signs the resulting ciphertext along with other parameters.

* **Parameters:**
  * `plaintext (int)`: Integer to encrypt.
  * `user_aes_key (bytes)`: AES key for encryption.
  * `sender (object)`: Ethereum-like sender object (with `address` attribute).
  * `contract (object)`: Ethereum-like contract object (with `address` attribute).
  * `function_selector (str)`: Function selector (in hex).
  * `signing_key (bytes)`: Signing key for signature.
* **Returns:**
  * `dict`: Contains the `ciphertext` and `signature`.

```python
def build_string_input_text(plaintext, user_aes_key, sender, contract, function_selector, signing_key)
```

Encrypts and signs string-based input data, breaking it into 8-byte chunks.

* **Parameters:**
  * `plaintext (str)`: String to encrypt.
  * `user_aes_key (bytes)`: AES key for encryption.
  * `sender (object)`: Ethereum-like sender object.
  * `contract (object)`: Ethereum-like contract object.
  * `function_selector (str)`: Function selector (in hex).
  * `signing_key (bytes)`: Signing key for signature.
* **Returns:**
  * `dict`: Contains the `ciphertext` and `signature`.

```python
def decrypt_uint(ciphertext, user_key)
```

Decrypts a ciphertext into an unsigned integer using AES.

* **Parameters:**
  * `ciphertext (CtUint)`: Ciphertext to decrypt (in integer format).
  * `user_key (bytes)`: AES key to use for decryption.
* **Returns:**
  * `int`: The decrypted integer.

```python
def decrypt_string(ciphertext, user_key)
```

Decrypts a ciphertext back into a string, handling multiple formats of ciphertext.

* **Parameters:**
  * `ciphertext (CtString)`: Ciphertext to decrypt, can be in event or state variable format.
  * `user_key (bytes)`: AES key to use for decryption.
* **Returns:**
  * `str`: The decrypted string.

```python
def generate_rsa_keypair()
```

Generates an RSA key pair for encryption and decryption.

* **Returns:**
  * `private_key_bytes (bytes)`: Serialized private key.
  * `public_key_bytes (bytes)`: Serialized public key.

```python
def decrypt_rsa(private_key_bytes, ciphertext)
```

Decrypts a ciphertext using RSA and a provided private key.

* **Parameters:**
  * `private_key_bytes (bytes)`: Private key used for decryption.
  * `ciphertext (bytes)`: Ciphertext to decrypt.
* **Returns:**
  * `plaintext (bytes)`: Decrypted plaintext.
