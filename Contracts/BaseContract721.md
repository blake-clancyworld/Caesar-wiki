**BaseContract721 & IBaseContract721**
The basis from which all ERC721 standard Clancy World tokens are sourced from.
# IBaseContract721
An interface contract, meaning it has no body, but contains empty functions that have their bodies filled out in the contracts that inherit from it. Only BaseContract721 should inherit from this contract interface directly.
## Functions
**mint()**
- The basic token creation and "minting" functionality to be interacted with by users.
- Returns the token id.

**mintTo(address to_)**
- The basic token creation and "minting" functionality, only able to be interacted with by the contract owner.
- The owner of the contract must include the recieving address for the token in this function call.

## Code
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IBaseContract721 {
    function mint() external returns (uint256);

    function mintTo(address to_) external returns (uint256);
}
```