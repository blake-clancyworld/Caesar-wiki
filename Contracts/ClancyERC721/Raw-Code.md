# IClancyERC721.sol
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IClancyERC721 {
    function mint() external returns (uint256);

    function mintMany(uint256 amount_) external returns (bytes32[] memory);

    function mintTo(address to_) external returns (uint256);

    function mintToMany(
        address[] memory _to
    ) external returns (bytes32[] memory);
}
```

# ClancyERC721.sol
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
import "./IClancyERC721.sol";

contract ClancyERC721 is
    ERC721URIStorage,
    ERC721Enumerable,
    ReentrancyGuard,
    Ownable,
    Pausable,
    IERC721Receiver,
    IClancyERC721
{
    using Counters for Counters.Counter;
    using Address for address;
    Counters.Counter internal _token_id_counter;
    uint256 private _max_supply;
    string private _baseURILocal;
    bool private _burn_enabled = false;
    bool private _public_mint_enabled = false;

    event MaxSupplyChanged(uint256);
    event BaseURIChanged(string, string);
    event BurnStatusChanged(bool);
    event Withdrawn(address, uint256);
    event PublicMintStatusChanged(bool);

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
        emit BaseURIChanged(_baseURILocal, baseURI_);
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
        emit PublicMintStatusChanged(_public_mint_enabled);
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
        return _safeMint(_msgSender());
    }

    /**
     * Mint many, callable by any.
     * @param amount_ The amount of tokens to mint.
     */
    function mintMany(
        uint256 amount_
    ) public virtual override whenNotPaused returns (bytes32[] memory) {
        require(_public_mint_enabled, "Public minting disabled.");
        require(amount_ > 0, "Count must be greater than zero.");
        require(
            amount_ <= getMaxSupply() - totalSupply(),
            "Amount exceeds max supply."
        );
        bytes32[] memory tokenIds = new bytes32[](amount_);
        uint256 i = 0;
        while (i < amount_) {
            tokenIds[i] = bytes32(_safeMint(_msgSender()));
            ++i;
        }
        return tokenIds;
    }

    /**
     * Mint to a specific address, callable by only owner.
     */
    function mintTo(address to) public override onlyOwner returns (uint256) {
        return _safeMint(to);
    }

    /**
     * Mint to many addresses, callable by only owner.
     * @param to The addresses to mint to.
     */
    function mintToMany(
        address[] memory to
    ) public override onlyOwner returns (bytes32[] memory) {
        require(to.length > 0, "No addresses to mint to.");
        bytes32[] memory tokenIds = new bytes32[](to.length);
        uint256 i = 0;
        while (i < to.length) {
            require(to[i] != address(0), "Invalid address.");
            tokenIds[i] = bytes32(_safeMint(to[i]));
            i++;
        }
        return tokenIds;
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
        emit BurnStatusChanged(_burn_enabled);
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
        payable(_msgSender()).transfer(balance);
        emit Withdrawn(_msgSender(), balance);
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
