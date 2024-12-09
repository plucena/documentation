# Basic Private Smart Contract

This guide will walk you through the process of taking a simple contract that tracks the sum of all numbers passed as arguments to the `add` function, and converting it into a privacy-enabled version.&#x20;

Copy and paste the following smart contract into a new file `Counter.sol`:

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

contract Counter {
    uint64 private _sum;
    
    function sum() public view returns (uint64) {
        return _sum;
    }

    function add(uint64 value) external {
        _sum += value;
    }
}
```

The next thing we will want to do is import the MPC Core library, which will help us make calls to the precompiled contracts that allow us to work with private data types.

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

import "@coti-io/coti-contracts/contracts/utils/mpc/MpcCore.sol";

contract Counter {
    uint64 private _sum;
    
    function sum() public view returns (uint64) {
        return _sum;
    }

    function add(uint64 value) external {
        _sum += value;
    }
}
```

Now we are ready to integrate private computations into our smart contract! \
To do this, we can change the types of `_sum` and `value` to their equivalent private data type (`utUint64` and `itUint64` respectively).

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

import "@coti-io/coti-contracts/contracts/utils/mpc/MpcCore.sol";

contract Counter {
    utUint64 private _sum;
    
    function sum() public view returns (utUint64) {
        return _sum;
    }

    function add(itUint64 calldata value) external {
        _sum += value;
    }
}
```

If you would try to compile this smart contract, you will encounter a compilation error. This is because `_sum` and `value` are of different types and they do not support the standard Solidity arithmetic operators. Let's update the code so that it uses the appropriate methods provided to us by the MPC Core library.

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

import "@coti-io/coti-contracts/contracts/utils/mpc/MpcCore.sol";

contract Counter {
    utUint64 private _sum;
    
    function sum() public view returns (utUint64) {
        return _sum;
    }

    function add(itUint64 calldata value) external {
        gtUint64 value_ = MpcCore.validateCiphertext(value);
        gtUint64 sum_ = MpcCore.onBoard(_sum.ciphertext);
        
        sum_ = MpcCore.add(sum_, value_);
        
        _sum = MpcCore.offBoardCombined(sum_, msg.sender);
    }
}
```

This code will compile successfully, but if we were to deploy it and then try to call `add`, we would find that the transaction would be reverted. This is because by default, the storage slot used by the `_sum` state variable is initialized to zero. This causes an error when we try to onboard it into the gcEVM, since it was not encrypted using the network AES key. Let's fix the issue by setting the value of `_sum` inside of the constructor.

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

import "@coti-io/coti-contracts/contracts/utils/mpc/MpcCore.sol";

contract Counter {
    utUint64 private _sum;
    
    constructor() {
        gtUint64 sum_ = MpcCore.setPublic64(0);
        
        _sum = MpcCore.offBoardCombined(sum_, msg.sender);
    }

    function sum() public view returns (utUint64) {
        return _sum;
    }

    function add(itUint64 calldata value) external {
        gtUint64 value_ = MpcCore.validateCiphertext(value);
        gtUint64 sum_ = MpcCore.onBoard(_sum.ciphertext);
        
        sum_ = MpcCore.add(sum_, value_);
        
        _sum = MpcCore.offBoardCombined(sum_, msg.sender);
    }
}
```

Lastly, since values that are encrypted with the network AES key are not very useful to dApps and users, we will update the `sum` function to return only the value encrypted with the user's AES key.

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

import "@coti-io/coti-contracts/contracts/utils/mpc/MpcCore.sol";

contract Counter {
    utUint64 private _sum;
    
    constructor() {
        gtUint64 sum_ = MpcCore.setPublic64(0);
        
        _sum = MpcCore.offBoardCombined(sum_, msg.sender);
    }

    function sum() public view returns (ctUint64) {
        return _sum.userCiphertext;
    }

    function add(itUint64 calldata value) external {
        gtUint64 value_ = MpcCore.validateCiphertext(value);
        gtUint64 sum_ = MpcCore.onBoard(_sum.ciphertext);
        
        sum_ = MpcCore.add(sum_, value_);
        
        _sum = MpcCore.offBoardCombined(sum_, msg.sender);
    }
}
```

In order to deploy the contract on the COTI network, you may consider using our [**Remix Plugin**](../tools/remix-plugin.md) or our [**Hardhat Template**](../tools/hardhat.md).
