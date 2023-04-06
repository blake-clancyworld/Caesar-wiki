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

--------------------------------
# Events
**MaxSupplyChanged**
```
event MaxSupplyChanged(uint256 indexed);
```
Emitted when the max supply is changed.

```
event BaseURIChanged(string indexed, string indexed);
```
Emitted when _baseURILocal is changed.
```
event BurnStatusChanged(bool indexed);
```
Emitted when burn status is changed.

-----------------

## Member Attributes

    /// @dev A counter for tracking the token ID
    Counters.Counter internal _tokenIdCounter;

    /// @dev A base URI for metadata of the token, to be concatenated with the token ID
    string internal _baseURILocal;

    /// @dev The maximum supply of the token
    uint256 internal _maxSupply;

    /// @dev The ceiling for the total supply of the token
    uint256 public constant SUPPLY_CEILING = 1_000_000;

    /// @dev A flag for enabling or disabling public minting
    bool private _publicMintEnabled = false;

    /// @dev A flag for enabling or disabling burning of tokens
    bool private _burnEnabled = false;

The ERC721 function that we override:
```
    /**
     * @dev Returns the base URI for this contract.
     * @return A string representing the base URI.
     */
    function _baseURI() internal view virtual override returns (string memory) {
        return _baseURILocal;
    }
```

# Modifiers
None.