# COTI MetaMask Snap

The COTI MetaMask Snap allows users to onboard their COTI accounts, add/decrypt on-chain tokens and NFTs , and interact with COTI dApps.

<figure><img src="../../.gitbook/assets/image (15).png" alt="" width="361"><figcaption><p>Coti Snap running inside MetaMask</p></figcaption></figure>

### Getting Started

#### 1. Install the Snap

As a user, you can start by installing the snap here:

[**COTI Snap**](https://snaps.metamask.io/snap/npm/coti/coti-snap)

Simply click the "**Add to MetaMask**" button on the upper right hand side of the screen and follow the prompts in your MetaMask wallet.

Once you click the "**Connect**" button, the prompt will go over the necessary permissions:

<figure><img src="../../.gitbook/assets/image (13).png" alt="" width="360"><figcaption><p>COTI snap installation screen</p></figcaption></figure>

The snap will request the following permissions:

1. **Access the internet**: allows the snap to interact with any website that calls it to make a request (you must still approve requests made to it).
2. **Display dialog windows in MetaMask**: allows the snap to show MetaMask pop-ups with custom text, input fields, and buttons to approve or reject an action.
3. **Display a custom screen**: allows the snap to display a custom home screen in MetaMask. When clicking on menu >> snaps >> COTI, the custom screen is shown.
4. **Store and manage its data on your device**: allows the snap to store, update, and retrieve data securely with encryption. Only the COTI snap can access this information.
5. **Access the Ethereum provider**: allows the snap to communicate with MetaMask directly, in order for it to read data from the blockchain and suggest messages and transactions.
6. **Use lifecycle hooks**: allows the snap to use lifecycle hooks to run code at specific times during its lifecycle. Lifecycle hooks are actions that run when the snap is installed or updated, for example, when the snap is installed initially, it allows the confirmation screen to be shown.
7. **Allow websites to communicate directly with COTI**: allows websites/dApps to send messages to the COTI snap and receive a response from the COTI snap. All interactions must be user-approved.

Once the snap is installed, the MetaMask site will offer to continue to COTI's companion dApp website to get started with the snap.

#### **2. Onboard Account (Generate AES Key)**

Once you are in the companion dApp site ([snap.coti.io](https://snap.coti.io)):

1. Click on the "**connect wallet**" button, follow the prompts on MetaMask.
2. Click on the "**Onboard account**" button. The prompt will indicate the contract you are interacting with. Click the "**Onboard**" button. Follow the prompts on MetaMask.\
   \
   **NOTE**: Because the data from the onboarding operation is encrypted and MetaMask does not know how to decrypt this data, the "_**Signature request**_" screen in MetaMask will show illegible characters. This is normal.\
   \
   ![](<../../.gitbook/assets/image (17).png>)\

3. Once your account is onboarded:
   1. Click the "**Reveal AES Key**" button to view your AES key.&#x20;
   2. Click the "**Delete**" button to delete your AES key.

{% hint style="info" %}
**NOTE**: Your AES key should be treated with the same sensitivity as your MetaMask private key. Users who can obtain this key will be able to decrypt sensitive data on the COTI network.
{% endhint %}

### Integrating your dApp with the COTI MetaMask Snap

If you are building a dApp on the COTI network and want it to interact with the COTI MetaMask snap, follow these steps:

1. Add `@metamask/providers` to your dApp.

```shell
yarn add @metamask/providers
```

2. Create a function to detect if the user has Metamask to use it as a provider. You can guide yourself with [this repo](https://github.com/MetaMask/template-snap-monorepo/blob/main/packages/site/src/utils/metamask.ts).
3. Create a `MetaMaskProvider` in your dapp, which will let us know if COTI AES key manager is installed in the user's wallet. You can guide yourself with [**this repo**](https://github.com/MetaMask/template-snap-monorepo/blob/main/packages/site/src/hooks/MetamaskContext.tsx).
4. Create the `useRequest` hook to interact with Metamask. You can guide yourself with [**this repo**](https://github.com/MetaMask/template-snap-monorepo/blob/main/packages/site/src/hooks/useRequest.ts).
5. Now, create a hook called `useInvokeKeyManager` to invoke the COTI MetaMask Snap.

```
export type InvokeKeyManagerParams = {
  method: string;
  params?: Record<string, unknown>;
};

export const useInvokeKeyManager = (snapId) => {
  const request = useRequest();

  /**
   * Invoke the requested method.
   *
   * @param params - The invoke params.
   * @param params.method - The method name.
   * @param params.params - The method params.
   * @returns The response.
   */
  const invokeKeyManager = async ({ method, params }: InvokeKeyManagerParams) =>
    request({
      method: 'wallet_invokeSnap',
      params: {
        snapId,
        request: params ? { method, params } : { method },
      },
    });

  return invokeKeyManager;
};
```

6. Optional - You can also create a hook that detects if COTI MetaMask Snap is installed or not.

```
export const useMetaMask = () => {
  const { provider, setInstalledSnap, installedSnap } = useMetaMaskContext();
  const request = useRequest();

    /**
   * Get the Snap informations from MetaMask.
   */
  const getSnap = async () => {
    const snaps = (await request({
      method: 'wallet_getSnaps',
    })) as GetSnapsResponse;

    setInstalledSnap(snaps[defaultSnapOrigin] ?? null);
  };

  useEffect(() => {
    const detect = async () => {
      if (provider) {
        await getSnap();
      }
    };

    detect().catch(console.error);
  }, [provider]);

  return { installedSnap, getSnap };
};
```

7. Done! Now if you want to encrypt or decrypt some data from your dApp, you can use something like this:

To encrypt

```
  const handleEncryptClick = async () => {
    const result = await invokeSnap({
      method: 'encrypt',
      params: { value: 'hello' },
    });
    if (result) {
      alert(result);
    }
  };
```

To decrypt

```
  const handleDecryptClick = async () => {
    const result = await invokeSnap({
      method: 'decrypt',
      params: {
        value: JSON.stringify({
          ciphertext: new Uint8Array([
            230, 250, 246, 145, 200, 66, 40, 179, 108, 187, 128, 135, 216, 44,
            32, 48,
          ]),
          r: new Uint8Array([
            67, 194, 73, 74, 131, 182, 125, 200, 112, 210, 211, 145, 192, 148,
            187, 11,
          ]),
        }),
      },
    });
    console.log('result', result);
    if (result) {
      alert(result);
    }
  };
```

Check the [**Metamask documentation**](https://docs.metamask.io/snaps/) for more information.
