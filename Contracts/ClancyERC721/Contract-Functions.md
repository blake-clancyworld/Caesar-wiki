# constructor

```
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
```

The constructor is overridden by all inherited child contracts.Child contracts can specify additional input parameters.

## Parameters
**name_**
The name of the contract.

**symbol_**
A short string, a reduced and sometimes vowel-less abbreviation of the contract name.

**baseURI_**
The base URI of the collection. If this is set to anything other than an empty string all tokens in the collection will have this string prefixed to their own unique URI
_Example 1:_
- baseURI_ is ""
  - Token 1 URI is set to "clancyworld.com"
  - Token 1 full URI is "clancyworld.com"

_Example 2:_
- baseURI_ is "clancyworld.com/"
  - Token 1 URI is set to "clancyworld.com"
  - Token 1 full URI is "clancyworld.com/clancyworld.com"
  - Token 2 URI is set to "2"
  - Token 2 full URI is "clancyworld.com/2"

**max_supply_**
The maximum supply of the token
_Can_ be changed later.

# setMaxSupply

```
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
```

Change the contracts max supply for its token. The input supply must be higher than zero, and higher than the current total supply.

# getMaxSupply
```
/**
 * Return the contracts max supply for the token.
 */
function getMaxSupply() public view returns (uint256) {
    return _max_supply;
}
```

Returns the private member attribute _max_supply.

# getTokenIdCounter
```
/**
  * Get the current token id counter.
  */
 function getTokenIdCounter() public view returns (uint256) {
     return _token_id_counter.current();
 }
```

Returns the private member attribute _token_id_counter.

# setBaseURI
```
/**
  * Set the base URI for all tokens
  * @param baseURI_ The base URI to set
  */
 function setBaseURI(string calldata baseURI_) public onlyOwner {
     require(bytes(baseURI_).length > 0, "BaseURI can not be empty");
     _baseURILocal = baseURI_;
 }
```
Sets the baseURI string for the collection.

# baseURI
```
 /**
  * Return the contracts baseURI
  */
 function baseURI() public view returns (string memory) {
     return _baseURI();
 }
```
Returns the overridden ERC721 _baseURI()

# setTokenURI
```
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
```
Sets the URI for the individual token.

# setPublicMintStatus
```
/**
 * Set the public minting flag.
 */
function setPublicMintStatus(bool open_mint) public onlyOwner {
    _public_mint_enabled = open_mint;
}
```
Enables or disables the public minting status.
This does not prevent the contract owner from minting tokens.

# getPublicMintStatus
```
 /**
  * Get the public minting flag.
  */
 function getPublicMintStatus() public view returns (bool) {
     return _public_mint_enabled;
 }
```
Returns the private member attribute __public_mint_enabled.

# mint
```
/**
 * Mint function, callable by any.
 */
function mint() public virtual override whenNotPaused returns (uint256) {
    require(_public_mint_enabled, "Public minting disabled.");
    return _safeMint(msg.sender);
}
```
The publicly accessible token minting function.
Requires that public minting be enabled and the contract not be paused.
Calls the _safeMint function.
- **Note:** There is no requirement to send funds to mint. If we allow public access to our chain, or our network is penetrated and the blockchain is accessed, an address could mint all remaining tokens.

# mintMany
```
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
```
The publicly accessible token minting function, to mint more than 1.
Calls the _safeMint function, for the input amount of times.
- **Note:** There is no requirement to send funds to mint. If we allow public access to our chain, or our network is penetrated and the blockchain is accessed, an address could mint all remaining tokens.

# mintTo
```
/**
 * Mint to a specific address, callable by only owner.
 */
function mintTo(
    address to
) public virtual override onlyOwner returns (uint256) {
    return _safeMint(to);
}
```
An owner only callable minting function.
This functions enables to mint tokens on demand for rewards, giveaways, challenges, etc.
Calls the _safeMint function.
- **Note**: There is no requirement to send funds to mint. If we allow public access to our chain, or our network is penetrated and the blockchain is accessed, an address could mint all remaining tokens.

# mintToMany
```
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
```
An owner only callable minting function.
This functions enables to mint 1 token to multiple users, on demand for rewards, giveaways, challenges, etc.
Calls the _safeMint function.
- **Note**: There is no requirement to send funds to mint. If we allow public access to our chain, or our network is penetrated and the blockchain is accessed, an address could mint all remaining tokens.


# _safeMint
```
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
```
A wrapper function for the implementation of OpenZeppelin's Contract Wizard ERC721 minting.

# setBurnStatus
```
/**
 * Set the burn enabled flag.
 */
function setBurnStatus(bool status) public onlyOwner {
    _burn_enabled = status;
}
```
Sets the burn status of the contract.
If disabled, tokens cannot be burned by anyone, including the owner.

# getBurnStatus
```
/**
 * Return the burn enabled flag.
 */
function getBurnStatus() public view returns (bool) {
    return _burn_enabled;
}
```
Returns the private member attribute _burn_enabled.

# burn
```
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
```
Burns a token.
Requires that burning be enabled, and the contract is not paused.

# pause
```
/**
 * Pause
 */
function pause() public onlyOwner {
    _pause();
}
```
Pause the contract, disabling the functions that implement Pausable.
Does not prevent all functions in the contract.
- **NOTE:** The contract can be rewritten to implemnt the ERC721Pausable contract, but because of the overlap with Pausable, we save some gas and space because we are not inheriting already implemented functions from ERC721Pausable and instead adding Pausables functionality manually to our functions.

# unpause
```
/**
 * Unpause
 */
function unpause() public onlyOwner {
    _unpause();
}
```
Unpauses the contract, resuming interaction for the functions that implement Pausable.

# withdraw
```
/**
 * Withdraw
 */
function withdraw() public onlyOwner {
    uint256 balance = address(this).balance;
    payable(msg.sender).transfer(balance);
}
```
Withdraws any ether sent to the contract, payable to the contract owner.

# getBalance
```
/**
 * Get the native token balance of the account.
 */
function getBalance() external view returns (uint256) {
    return address(this).balance;
}
```
Returns the quantity of ether owned by the contract itself.