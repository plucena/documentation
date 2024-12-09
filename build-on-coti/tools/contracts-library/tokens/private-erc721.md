# Private ERC721

Private ERC721 is an abstract implementation of the [ERC721 Non-Fungible Token (NFT) Standard](https://eips.ethereum.org/EIPS/eip-721). It includes essential features of the standard, such as token ownership, approval, transfers, and safe transfers. This contract implements key components of the ERC721 standard while maintaining support for token metadata, but without fully implementing the metadata extension.

## Core

### Usage

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

import "@coti-io/coti-contracts/contracts/token/PrivateERC721/PrivateERC721.sol";

contract MyNFT is PrivateERC721 {
    constructor() PrivateERC721("Private NFT", "PNFT") {}
}
```

### Functions

```solidity
constructor(string memory name_, string memory symbol_)
```

```solidity
function supportsInterface(bytes4 interfaceId) view returns (bool)
```

```solidity
function balanceOf(address owner) view returns (uint256)
```

```solidity
function ownerOf(uint256 tokenId) view returns (address)
```

```solidity
function name() view returns (string memory)
```

```solidity
function symbol() view returns (string memory)
```

```solidity
function approve(address to, uint256 tokenId)
```

```solidity
function getApproved(uint256 tokenId) view returns (address)
```

```solidity
function setApprovalForAll(address operator, bool approved)
```

```solidity
function isApprovedForAll(address owner, address operator) view returns (bool)
```

```solidity
function transferFrom(address from, address to, uint256 tokenId)
```

```solidity
function safeTransferFrom(address from, address to, uint256 tokenId)
```

```solidity
function safeTransferFrom(address from, address to, uint256 tokenId, bytes memory data)
```

```solidity
function _ownerOf(uint256 tokenId) view returns (address)
```

```solidity
function _getApproved(uint256 tokenId) view returns (address)
```

```solidity
function _isAuthorized(address owner, address spender, uint256 tokenId) view returns (bool)
```

```solidity
function _checkAuthorized(address owner, address spender, uint256 tokenId) view
```

```solidity
function _increaseBalance(address account, uint128 value)
```

```solidity
function _update(address to, uint256 tokenId, address auth) returns (address)
```

```solidity
function _mint(address to, uint256 tokenId)
```

```solidity
function _safeMint(address to, uint256 tokenId)
```

```solidity
function _safeMint(address to, uint256 tokenId, bytes memory data)
```

```solidity
function _burn(uint256 tokenId)
```

```solidity
function _transfer(address from, address to, uint256 tokenId)
```

```solidity
function _safeTransfer(address from, address to, uint256 tokenId)
```

```solidity
function _safeTransfer(address from, address to, uint256 tokenId, bytes memory data)
```

```solidity
function _approve(address to, uint256 tokenId, address auth)
```

```solidity
function _approve(address to, uint256 tokenId, address auth, bool emitEvent)
```

```solidity
function _setApprovalForAll(address owner, address operator, bool approved)
```

```solidity
function _requireOwned(uint256 tokenId) view returns (address)
```

### Events

```solidity
event Transfer(address indexed from, address indexed to, uint256 indexed tokenId)
```

```solidity
event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId)
```

```solidity
event ApprovalForAll(address indexed owner, address indexed operator, bool approved)
```

### Errors

```solidity
error ERC721InvalidOwner(address owner)
```

```solidity
error ERC721NonexistentToken(uint256 tokenId)
```

```solidity
error ERC721IncorrectOwner(address sender, uint256 tokenId, address owner)
```

```solidity
error ERC721InvalidSender(address sender)
```

```solidity
error ERC721InvalidReceiver(address receiver)
```

```solidity
error ERC721InsufficientApproval(address operator, uint256 tokenId)
```

```solidity
error ERC721InvalidApprover(address approver)
```

```solidity
error ERC721InvalidOperator(address operator)
```

## Extensions

### PrivateERC721URIStorage

#### Usage

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

import "@coti-io/coti-contracts/contracts/token/PrivateERC721/extensions/PrivateERC721URIStorage.sol";

contract MyNFT is PrivateERC721URIStorage {
    constructor() PrivateERC721("Private NFT", "PNFT") {}
}
```

#### Functions

```solidity
function tokenURI(uint256 tokenId) view returns (ctString memory)
```

```solidity
function _setTokenURI(address to, uint256 tokenId, itString calldata itTokenURI)
```

```solidity
function _update(address to, uint256 tokenId, address auth) returns (address)
```

#### Errors

```solidity
error ERC721URIStorageNonMintedToken(uint256 tokenId)
```
