**ClancyERC721 & IClancyERC721**
The basis from which all ERC721 standard Clancy World tokens are sourced from.
# IClancyERC721
An interface contract, meaning it has no body, but contains empty functions that have their bodies filled out in the contracts that inherit from it. Only ClancyERC721 should inherit from this contract interface directly.

## Functions
**mint()**
- The basic token creation functionality to be interacted with by users.
- Returns the token id.

## Code
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

interface IClancyERC721 {
    // Errors
    error PublicMintDisabled();
    error BurnDisabled();
    error NotApprovedOrOwner();
    error MaxSupply_AboveCeiling();
    error MaxSupply_CannotBeDecreased();
    error MaxSupply_LowerThanCurrentSupply();
    error MaxSupply_LTEZero();
    error MaxSupply_Reached();

    // Events
    event MaxSupplyChanged(uint256 indexed);
    event BaseURIChanged(string indexed, string indexed);
    event BurnStatusChanged(bool indexed);

    // Functions
    function mint() external returns (uint256);
}

```

# ClancyERC721
The foundation of all ClancyWorld's ERC721 tokens.
This contract implements     Ownable,
    ERC721Enumerable,
    Pausable,
    IClancyERC721,
    IERC721Receiver
## ERC721URIStorage
ERC721 token with storage based token URI management.

## ERC721Enumerable
This implements an optional extension of {ERC721} defined in the EIP that adds enumerability of all the token ids in the contract as well as all token ids owned by each account.
- Note: This is a gas heavy extension. It is helpful for easy identification of a users owned assets.

## ReentrancyGuard
Contract module that helps prevent reentrant calls to a function.
https://hackernoon.com/hack-solidity-reentrancy-attack

## Ownable
Contract module which provides a basic access control mechanism, where there is an account (an owner) that can be granted exclusive access to specific functions.

## Pausable
Contract module which allows us to implement an emergency stop mechanism that can be triggered by an authorized account, on specific functions.
This is not inherently a failsafe.
- It requires the function implementation of the function whenNotPaused()
- It requires the contract owner to have set the paused state to true.

## IERC721Receiver
Interface for any contract that wants to support safeTransfers from ERC721 asset contracts. This is essentially a way to for the contract to own assets, and allow "hard" staking of assets. Without this function, any assets sent to the contract (marking the contract as the asset owner) would be lost.

## Fallback Funtions
**recieve**
```
/**
 * Fallback Recieve function
 */
receive() external payable {}
```
The receive function is executed on a call to the contract with empty calldata. This is the function that is executed on plain Ether transfers (e.g. via .send() or .transfer()). If no such function exists, but a payable fallback function exists, the fallback function will be called on a plain Ether transfer. If neither a receive Ether nor a payable fallback function is present, the contract cannot receive Ether through a transaction that does not represent a payable function call and throws an exception.