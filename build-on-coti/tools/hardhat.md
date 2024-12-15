# Hardhat

The easiest way to get started with writing smart contracts on COTI is to clone the [**COTI Hardhat Template**](https://github.com/coti-io/coti-hardhat-template). This template is a simple [**Hardhat**](https://hardhat.org/) project that includes all the configurations and packages needed to connect to the Testnet and integrate COTI's privacy features into your own smart contracts.

{% hint style="info" %}
Since the local Hardhat network does not include the precompiled contracts needed for computations on private data types, the **only** way to test contracts that use these features is by running your test scripts on the COTI Testnet.
{% endhint %}

Let's start by cloning the repository:

```bash
git clone https://github.com/coti-io/coti-hardhat-template
```

Before we can continue with exploring the repository, we have to install the dependencies:

```bash
npm install
```

The repository includes a simple privacy-enabled smart contract (`PrivateStorage.sol`) which, as the name suggests, accepts encrypted inputs and stores them on-chain using the user's [AES encryption key](../../how-coti-works/advanced-topics/aes-keys.md). There is also a short test suite included in the template.

To run the test suite, execute the following command in your terminal:

```bash
npx hardhat test
```

On your first time running the test suite, you will see the following message printed to the console:

```bash
1) PrivateStorage
       Deployment
         Should set the value of the encrypted number:
     Error: Created new random accounts <ACCCOUNT_ADDRESS>. Please use faucet to fund it.
```

To fund the newly created account, head over to the [**faucet**](https://faucet.coti.io/) and send a message to the both in the following format:

```bash
testnet <ACCCOUNT_ADDRESS>
```

Now that you have funded your account, you can run the test suite again:

```bash
npx hardhat test
```
