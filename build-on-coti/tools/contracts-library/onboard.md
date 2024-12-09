# Onboard

The AccountOnboard contract is responsible for onboarding user accounts to the system, leveraging RSA public keys and AES encryption. During onboarding, the user's AES encryption key is emitted in an event in encrypted form. This contract interacts with the MPC Core library to retrieve the user key.

## Functions

```solidity
function onboardAccount(bytes calldata publicKey, bytes calldata signedEK)
```

## Events

```solidity
event AccountOnboarded(address indexed _from, bytes userKey1, bytes userKey2)
```
