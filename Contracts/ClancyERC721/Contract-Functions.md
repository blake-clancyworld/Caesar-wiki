# **Parameters**
**name_**
The name of the contract.

**symbol_**
A short string, a reduced and sometimes vowel-less abbreviation of the contract name.

**max_supply_**
The maximum supply of the token
_Can_ be changed later.

**baseURILocal_**
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

-----------------------------
# **Constructor**
```
    constructor(
        string memory name_,
        string memory symbol_,
        uint256 max_supply_,
        string memory baseURILocal_
    ) ERC721(name_, symbol_) {
        _maxSupply = max_supply_;
        _baseURILocal = baseURILocal_;
    }

```

The constructor is overridden by all inherited child contracts.Child contracts can specify additional input parameters.

-------------------
# **Functions**
## pause
```
    /**
     * @dev Pauses the contract.
     *
     * Requirements:
     * - The contract must not already be paused.
     * - Can only be called by the owner of the contract.
     */
    function pause() public onlyOwner {
        _pause();
    }
```
Pause the contract, disabling the functions that implement Pausable.
Does not prevent all functions in the contract.

## unpause
```
    /**
     * @dev Unpauses the contract.
     *
     * Requirements:
     * - The contract must be paused.
     * - Can only be called by the owner of the contract.
     */
    function unpause() public onlyOwner {
        _unpause();
    }
```
Unpauses the contract, resuming interaction for the functions that implement Pausable.

## mint
```
    /**
     * @dev Mints a new token and assigns it to the caller's address.
     *
     * Requirements:
     * - Public minting must be enabled.
     * - The contract must not be paused.
     *
     * @return The id of the newly minted token.
     */
    function mint() public virtual override returns (uint256) {
        if (!_publicMintEnabled) revert PublicMintDisabled();
        return uint256(clancyMint(_msgSender()));
    }
```
Main public minting function.

## setBurnStatus
```

    /**
     * @dev Sets the burn status of the contract.
     *
     * Requirements:
     * - Can only be called by the owner of the contract.
     *
     * @param status A boolean indicating whether or not burning is enabled.
     */
    function setBurnStatus(bool status) public onlyOwner {
        _burnEnabled = status;
        emit BurnStatusChanged(status);
    }
```
Burn status is independent of pausable.
### Examples
- Burn Enabled, Paused
  - Cannot burn.
- Burn Disabled, Not Paused
  - Cannot burn. 
- Burn Enabled, Not Paused
  - Can be burned.


## burn
```
    /**
     * @dev Burns a token.
     *
     * Requirements:
     * - Burning must be enabled.
     * - The token must exist.
     * - The caller must either own the token or be approved to burn it.
     * - The contract must not be paused.
     *
     * @param tokenId The ID of the token to be burned.
     */
    function burn(uint256 tokenId) public virtual whenNotPaused {
        if (!_burnEnabled) revert BurnDisabled();
        if (!_isApprovedOrOwner(_msgSender(), tokenId))
            revert NotApprovedOrOwner();
        _burn(tokenId);
    }
```
Burn function that calls the ERC721 burn function. Not required to check isApprovedOrOwner as the ERC721 _burn function include this, but defining our own Error is better for readability and gas efficiency.

## getBurnStatus
```
    /**
     * @dev Returns the burn status of the contract.
     *
     * @return A boolean indicating whether or not burning is currently enabled.
     */
    function getBurnStatus() public view returns (bool) {
        return _burnEnabled;
    }
```
Gets the burn status.

## setPublicMintStatus
```
    /**
     * @dev Sets the public minting status of the Clancy ERC721 token.
     *
     * Requirements:
     * - The caller must be the owner of the contract.
     *
     * @param status The new public minting status.
     */
    function setPublicMintStatus(bool status) public onlyOwner {
        _publicMintEnabled = status;
    }
```
Sets public mint status. Inherited contracts can still call clancyMint.

## setMaxSupply
```
    /**
     * @dev Sets the maximum supply of the Clancy ERC721 token.
     *
     * Requirements:
     * - The caller must be the owner of the contract.
     * - The increased supply must be greater than 0.
     * - The increased supply must be greater than the current maximum supply.
     * - The increased supply must not exceed the supply ceiling.
     *
     * Emits a {MaxSupplyChanged} event indicating the updated maximum supply.
     *
     * @param increasedSupply The new maximum supply.
     */
    function setMaxSupply(uint256 increasedSupply) public onlyOwner {
        if (increasedSupply <= 0) revert MaxSupply_LTEZero();
        if (increasedSupply < totalSupply())
            revert MaxSupply_LowerThanCurrentSupply();
        if (increasedSupply > SUPPLY_CEILING) revert MaxSupply_AboveCeiling();

        _maxSupply = increasedSupply;

        emit MaxSupplyChanged(increasedSupply);
    }
```
Sets max supply, emits an event and has revert conditions.

## setBaseURI
```
    /**
     * @dev Sets the base URI for the Clancy ERC721 token metadata.
     *
     * Requirements:
     * - The caller must be the owner of the contract.
     *
     * Emits a {BaseURIChanged} event indicating the updated base URI.
     *
     * @param baseURI_ The new base URI for the token metadata.
     */
    function setBaseURI(string calldata baseURI_) public onlyOwner {
        string memory existingBaseURI = _baseURILocal;
        _baseURILocal = baseURI_;
        emit BaseURIChanged(existingBaseURI, _baseURILocal);
    }
```
Sets the base uri.
Should be an internally hosted resource.

## getPublicMintStatus
```
    /**
     * @dev Returns the public mint status of this contract.
     * @return A boolean indicating whether public minting is currently enabled or disabled.
     */
    function getPublicMintStatus() public view returns (bool) {
        return _publicMintEnabled;
    }
```
Gets the public mint status.

## baseURI
```
    /**
     * @dev Returns the base URI for all tokens.
     * @return A string representing the base URI.
     */
    function baseURI() public view virtual returns (string memory) {
        return _baseURI();
    }
```
Calls our defined _baseURI that overrides the default ERC721 _baseURI.

## getMaxSupply
```
    /**
     * @dev Returns the maximum supply for this token.
     * @return An unsigned integer representing the maximum supply.
     */
    function getMaxSupply() public view returns (uint256) {
        return _maxSupply;
    }
```
Gets the max supply.

## getTokenIdCounter
```
    /**
     * @dev Returns the total number of tokens in existence.
     *      Burned tokens will not reduce this number, it will only increase.
     * @return uint256 representing the total number of tokens in existence.
     */
    function getTokenIdCounter() public view returns (uint256) {
        return _tokenIdCounter.current();
    }
```
Gets the token id counter.

## clancyMint
```
    /**
     * @dev Mints a new Clancy ERC721 token and assigns it to the specified address.
     *
     * Requirements:
     * - The contract must not be paused.
     * - The maximum supply has not been reached.
     *
     * @param to The address to assign the newly minted token to.
     * @return tokenId - The ID of the newly minted token.
     */
    function clancyMint(
        address to
    ) internal whenNotPaused returns (uint256 tokenId) {
        if (_tokenIdCounter.current() >= _maxSupply) revert MaxSupply_Reached();
        _tokenIdCounter.increment();
        tokenId = _tokenIdCounter.current();
        _safeMint(to, tokenId);
        return tokenId;
    }
```
Internal minting wrapper.