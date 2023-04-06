## _baseURI
```
    /**
     * @dev Returns the base URI for this contract.
     * @return A string representing the base URI.
     */
    function _baseURI() internal view virtual override returns (string memory) {
        return _baseURILocal;
    }
```
Overrides ERC721s function.


## onERC721Recieved
```
    /**
     * @dev See {IERC721Receiver-onERC721Received}.
     */
    function onERC721Received(
        address,
        address,
        uint256,
        bytes memory
    ) public virtual returns (bytes4) {
        return this.onERC721Received.selector;
    }
```
Function required for the contract to hold ERC721 tokens.