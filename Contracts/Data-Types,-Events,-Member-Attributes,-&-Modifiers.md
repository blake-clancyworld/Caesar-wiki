# Data Types
The contract uses Open Zeppelins library for Address and Counter safety and helper functionality.

```
using Counters for Counters.Counter;
using Address for address;
```
## Address
Collection of functions related to the address type.

## Counters
Provides counters that can only be incremented, decremented or reset. This can be used e.g. to track the number of elements in a mapping, issuing ERC721 ids, or counting request ids.

# Events
**MaxSupplyChanged**
```
event MaxSupplyChanged(uint256);
```
Emitted when the max supply is changed.

## Member Attributes

```
Counters.Counter internal _token_id_counter;
uint256 private _max_supply;
string private _baseURILocal;
bool private _burn_enabled = false;
bool private _public_mint_enabled = false;
```
**Counters.Counter internal _token_id_counter**
A counter for the indexing of a tokens id.
- This number should never decrement.

```
uint256 private _max_supply
```
The maximum amount of tokens to be issued for the contract.
- It can always be increased.
- It can never be set to a number less than the current token id.

```
string private _baseURILocal
```
Required for a contract that inherits from ERC721 to return the base URI for all tokens.

**Example:**
- If _baseURILocal is "http://clancyworld.com/", all tokens would have a baseURI of "http://clancyworld.com/".
  - Any further URI data would be concatenated at the end fo this base string.

The ERC721 function that we override:
```
/**
 * @dev Base URI for computing {tokenURI}. If set, the resulting URI for each
 * token will be the concatenation of the `baseURI` and the `tokenId`. Empty
 * by default, can be overridden in child contracts.
 */
function _baseURI() internal view virtual returns (string memory) {
     return "";
}
```

**bool private _burn_enabled = false**
A flag to permit or deny true burning of a token. By default it is set to false and must be set to true for users to burn.

**bool private _public_mint_enabled = false**
A flag to permit or deny any user to mint a token. By default it is set to false and must be to true for users to mint.
- Even if this flag is set to false, tokens can still be minted, but only by the contract owner.

# Modifiers
None.