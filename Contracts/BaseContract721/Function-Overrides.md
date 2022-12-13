# onERC721Recieved

```
//#region Interface Implementation
function onERC721Received(
    address,
    address,
    uint256,
    bytes memory
) public virtual returns (bytes4) {
    return this.onERC721Received.selector;
}
```
Interface for any contract that wants to support safeTransfers from ERC721 asset contracts.

# _baseUri

```
function _baseURI() internal view override returns (string memory) {
    return _baseURILocal;
}
```
# tokenUri

```
function tokenURI(
    uint256 tokenId
) public view override(ERC721, ERC721URIStorage) returns (string memory) {
    return super.tokenURI(tokenId);
}
```

# _burn

```
function _burn(
    uint256 tokenId
) internal override(ERC721, ERC721URIStorage) {
    super._burn(tokenId);
}
```

# _beforeTokenTransfer

```
function _beforeTokenTransfer(
    address from,
    address to,
    uint256 tokenId,
    uint256 batchSize
) internal override(ERC721, ERC721Enumerable) whenNotPaused {
    super._beforeTokenTransfer(from, to, tokenId, batchSize);
}
```

# supportsInterface

```
function supportsInterface(
    bytes4 interfaceId
) public view override(ERC721, ERC721Enumerable) returns (bool) {
    return super.supportsInterface(interfaceId);
}
```
[https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[EIP section]]()