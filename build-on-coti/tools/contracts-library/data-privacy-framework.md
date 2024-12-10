# Data Privacy Framework

The [**Data Privacy Framework**](https://github.com/coti-io/coti-contracts/blob/main/contracts/access/DataPrivacyFramework/DataPrivacyFramework.sol) is an abstract Solidity contract designed to manage conditions and operations related to data privacy. The contract handles operations such as user permissions, time-bound constraints, and condition validation based on various keys and parameters.

## Core

### Usage

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

import "@coti-io/coti-contracts/contracts/access/DataPrivacyFramework/DataPrivacyFramework.sol";

contract MyContract is DataPrivacyFramework {
    constructor() DataPrivacyFramework(false, false) {}
}
```

### Types

```solidity
struct InputData {
        address caller;
        string operation;
        bool active;
        uint256 timestampBefore;
        uint256 timestampAfter;
        bool falseKey;
        bool trueKey;
        uint256 uintParameter;
        address addressParameter;
        string stringParameter;
}
```

```solidity
struct Condition {
        uint256 id; // numeric ID of the condition
        address caller; // caller associated with this condition
        string operation; // operation associated with this condition
        bool active; // indicates if the permission is active
        bool falseKey; // causes the condition to never be satisfied
        bool trueKey; // causes the permission to always be satisfied (but has lower priority than falseKey)
        uint256 timestampBefore; // condition is valid before this timestamp
        uint256 timestampAfter; // condition is valid after this timestamp
        uint256 uintParameter; // parameter of type uint256 used for verifying if the caller has permission to perform the computation
        address addressParameter; // parameter of type address used for verifying if the caller has permission to perform the computation
        string stringParameter;// parameter of type string used for verifying if the caller has permission to perform the computation
}
```

```solidity
enum ParameterType {
        None,
        UintParam,
        AddressParam,
        StringParam
}
```

### Modifiers

```solidity
modifier onlyAllowedUserOperation(string memory operation, uint256 uintParameter, address addressParameter, string memory stringParameter)
```

### Functions

```solidity
constructor(bool addressDefaultPermission_, bool operationDefaultPermission_)
```

```solidity
function getConditionsCount() view returns (uint256)
```

```solidity
function getConditions(uint256 startIdx, uint256 chunkSize) view returns (Condition[] memory)
```

```solidity
function isOperationAllowed(address caller, string calldata operation) view returns (bool)
```

```solidity
function isOperationAllowed(address caller, string calldata operation, uint256 uintParameter) view returns (bool)
```

```solidity
function isOperationAllowed(address caller, string calldata operation, address addressParameter) view returns (bool)
```

```solidity
function isOperationAllowed(address caller, string calldata operation, string calldata stringParameter) view returns (bool)
```

```solidity
function setAddressDefaultPermission(bool defaultPermission) returns (bool)
```

```solidity
function setOperationDefaultPermission(bool defaultPermission) returns (bool)
```

```solidity
function addAllowedOperation(string memory operation) returns (bool)
```

```solidity
function removeAllowedOperation(string calldata operation) returns (bool)
```

```solidity
function addRestrictedOperation(string memory operation) returns (bool)
```

```solidity
function removeRestrictedOperation(string calldata operation) returns (bool)
```

```solidity
function setPermission(InputData memory inputData) returns (bool)
```

```solidity
function _isOperationAllowed(address caller, string calldata operation, ParameterType parameterType, uint256 uintParameter, address addressParameter, string memory stringParameter) view returns (bool)
```

```solidity
function _evaluateCondition(Condition memory condition, ParameterType parameterType, uint256 uintParameter, address addressParameter, string memory stringParameter) view returns (bool)
```

## Extensions

### DataPrivacyFrameworkMpc

#### Usage

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

import "@coti-io/coti-contracts/contracts/access/DataPrivacyFramework/extensions/DataPrivacyFrameworkMpc.sol";

contract MyContract is DataPrivacyFrameworkMpc {
    constructor() DataPrivacyFrameworkMpc(false, false) {}
}
```

#### Functions

{% hint style="info" %}
Since each numeric private data type supports the same functions, we have chosen to list only the functions pertaining to the gtUint64 type. See [**DataPrivacyFrameworkMpc.sol**](https://github.com/coti-io/coti-contracts/blob/smiller-coti/testnet/contracts/access/DataPrivacyFramework/extensions/DataPrivacyFrameworkMpc.sol) for the full list of supported functions.
{% endhint %}

```solidity
constructor(bool addressDefaultPermission_, bool operationDefaultPermission_)
```

```solidity
function add(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function sub(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function mul(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function div(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function rem(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function and(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function or(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function xor(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function eq(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtBool)
```

```solidity
function ne(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtBool)
```

```solidity
function ge(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtBool)
```

```solidity
function gt(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtBool)
```

```solidity
function le(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtBool)
```

```solidity
function lt(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtBool)
```

```solidity
function min(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function max(gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```

```solidity
function decrypt(gtUint64 a, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (uint64)
```

```solidity
function mux(gtBool bit, gtUint64 a, gtUint64 b, uint256 uintParameter, address addressParameter, string calldata stringParameter) returns (gtUint64)
```
