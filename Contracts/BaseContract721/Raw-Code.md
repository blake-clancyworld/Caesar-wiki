# IBaseContract.sol
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IBaseContract721 {
    function mint() external returns (uint256);

    function mintTo(address to_) external returns (uint256);
}
```

# BaseContract.sol
````
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import "@openzeppelin/contracts/token/ERC721/IERC721Receiver.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";
import "@openzeppelin/contracts/security/Pausable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
import "@openzeppelin/contracts/utils/Address.sol";
import "./IBaseContract721.sol";

contract BaseContract721 is
    ERC721URIStorage,
    ERC721Enumerable,
    ReentrancyGuard,
    Ownable,
    Pausable,
    IERC721Receiver,
    IBaseContract721
{
    using Counters for Counters.Counter;
    using Address for address;
    Counters.Counter internal _token_id_counter;
    uint256 private _max_supply;
    string private _baseURILocal;
    bool private _burn_enabled = false;
    bool private _public_mint_enabled = false;

    event MaxSupplyChanged(uint256);

    constructor(
        string memory name_,
        string memory symbol_,
        string memory baseURI_,
        uint256 max_supply_
    ) ERC721(name_, symbol_) {
        require(bytes(name_).length > 0, "Name cannot be empty.");
        require(bytes(symbol_).length > 0, "Symbol cannot be empty.");
        require(bytes(baseURI_).length > 0, "BaseURI cannot be empty.");
        require(max_supply_ > 0, "Max supply cannot be zero.");
        _max_supply = max_supply_;
        _baseURILocal = baseURI_;
    }

    /**
     * Set max supply to the input value.
     * Cannot be lower than the current total supply.
     * @param supply The value to set the max supply to.
     */
    function setMaxSupply(uint256 supply) public onlyOwner {
        require(supply > 0, "Max supply cannot be zero.");
        require(
            supply >= totalSupply(),
            "Max supply cannot be less than total supply."
        );
        _max_supply = supply;

        emit MaxSupplyChanged(_max_supply);
    }

    /**
     * Return the contracts max supply for the token.
     */
    function getMaxSupply() public view returns (uint256) {
        return _max_supply;
    }

    /**
     * Get the current token id counter.
     */
    function getTokenIdCounter() public view returns (uint256) {
        return _token_id_counter.current();
    }

    /**
     * Set the base URI for all tokens
     * @param baseURI_ The base URI to set
     */
    function setBaseURI(string calldata baseURI_) public onlyOwner {
        require(bytes(baseURI_).length > 0, "BaseURI can not be empty");
        _baseURILocal = baseURI_;
    }

    /**
     * Return the contracts baseURI
     */
    function baseURI() public view returns (string memory) {
        return _baseURI();
    }

    /**
     * setTokenURI
     * @param tokenId The token to set the URI for.
     * @param tokenURI_ The URI to set.
     */
    function setTokenURI(
        uint256 tokenId,
        string memory tokenURI_
    ) public onlyOwner {
        _setTokenURI(tokenId, tokenURI_);
    }

    /**
     * Set the public minting flag.
     */
    function setPublicMintStatus(bool open_mint) public onlyOwner {
        _public_mint_enabled = open_mint;
    }

    /**
     * Get the public minting flag.
     */
    function getPublicMintStatus() public view returns (bool) {
        return _public_mint_enabled;
    }

    /**
     * Mint function, callable by any.
     */
    function mint() public virtual override whenNotPaused returns (uint256) {
        require(_public_mint_enabled, "Public minting disabled.");
        return _safeMint(msg.sender);
    }

    /**
     * Mint to a specific address, callable by only owner.
     */
    function mintTo(
        address to
    ) public virtual override onlyOwner returns (uint256) {
        return _safeMint(to);
    }

    /**
     * Implemntation of ERC721 minting.
     */
    function _safeMint(
        address to
    ) private whenNotPaused returns (uint256 tokenId) {
        require(
            _token_id_counter.current() < _max_supply,
            "Max supply reached."
        );
        _token_id_counter.increment();
        tokenId = _token_id_counter.current();
        _safeMint(to, tokenId);
        return tokenId;
    }

    /**
     * Set the burn enabled flag.
     */
    function setBurnStatus(bool status) public onlyOwner {
        _burn_enabled = status;
    }

    /**
     * Return the burn enabled flag.
     */
    function getBurnStatus() public view returns (bool) {
        return _burn_enabled;
    }

    /**
     * Burn a token.
     */
    function burn(uint256 tokenId) public virtual whenNotPaused {
        require(_burn_enabled, "Burn is not enabled.");
        require(_exists(tokenId), "ERC721: token does not exist.");
        require(
            _isApprovedOrOwner(_msgSender(), tokenId),
            "Ownable: Caller is not owner or approved."
        );
        _burn(tokenId);
    }

    /**
     * Pause
     */
    function pause() public onlyOwner {
        _pause();
    }

    /**
     * Unpause
     */
    function unpause() public onlyOwner {
        _unpause();
    }

    /**
     * Fallback Recieve function
     */
    receive() external payable {}

    /**
     * Withdraw
     */
    function withdraw() public onlyOwner {
        uint256 balance = address(this).balance;
        payable(msg.sender).transfer(balance);
    }

    /**
     * Get the native token balance of the account.
     */
    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }

    //#region Interface Implementation
    function onERC721Received(
        address,
        address,
        uint256,
        bytes memory
    ) public virtual returns (bytes4) {
        return this.onERC721Received.selector;
    }

    //#endregion

    //#region Overrides
    function _baseURI() internal view override returns (string memory) {
        return _baseURILocal;
    }

    function tokenURI(
        uint256 tokenId
    ) public view override(ERC721, ERC721URIStorage) returns (string memory) {
        return super.tokenURI(tokenId);
    }

    function _burn(
        uint256 tokenId
    ) internal override(ERC721, ERC721URIStorage) {
        super._burn(tokenId);
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 tokenId,
        uint256 batchSize
    ) internal override(ERC721, ERC721Enumerable) whenNotPaused {
        super._beforeTokenTransfer(from, to, tokenId, batchSize);
    }

    function supportsInterface(
        bytes4 interfaceId
    ) public view override(ERC721, ERC721Enumerable) returns (bool) {
        return super.supportsInterface(interfaceId);
    }
    //#endregion
}
```
