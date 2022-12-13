# setMomentsContract
```
/**
 * Set the contract for the moments contained within the pack.
 * @param momentsContract The address of the moments contract.
 */
function setMomentsContract(address momentsContract) public onlyOwner {
    require(
        momentsContract != address(0),
        "Moments contract cannot be the zero address."
    );
    require(momentsContract.isContract(), "Address is not a contract.");
    _momentsContract = Moments(payable(momentsContract));
}
```
Sets the Moment contract for the Pack.
- **Notes:**
  - Only one moment contract can be set at a time.
  - Packs in the same series can be set to different Moments contracts.

# getMomentsContract
```
  /**
   * Get the moments contract.
   */
  function getMomentsContract() public view returns (Moments) {
      return _momentsContract;
  }
```
Returns the private member attribute **_momentsContract**.

# setMomentsPerPack
```
/**
 * Set how many moments are contained (minted) within the pack.
 * @param moments_per_pack The number of moments per pack.
 */
function setMomentsPerPack(uint256 moments_per_pack) public onlyOwner {
    require(
        moments_per_pack > 0,
        "Moments per pack must be greater than zero."
    );
    _moments_per_pack = moments_per_pack;
}
```
Sets the amount of moments to be minted to the address burning and opening their pack.
By default this is set to 3.

- **Notes**:
  - There is no maximum on the amount of moments that can be minted, but there is a minimum of 0.

# getMomentsPerPack
```
 /**
   * Returns the amount of moments created when a pack is opened.
   */
  function getMomentsPerPack() public view returns (uint256) {
      return _moments_per_pack;
  }
```
Returns the private member attribute _moments_per_pack.

# mintMany
```
/**
 * Mint many tokens.
 * @param amount The amount of tokens to mint.
 */
function mintMany(uint256 amount) external {
    require(
        _momentsContract != Moments(payable(address(0))),
        "Moments contract not set."
    );
    require(amount > 0, "Amount must be greater than zero.");
    require(
        amount <= getMaxSupply() - totalSupply(),
        "Amount must be less than or equal to the remaining supply."
    );
    for (uint256 i = 0; i < amount; i++) {
        mint();
    }
}
```
An externally callable function to mint multiple tokens.
- **Note:**
  - There is no requirement to send funds to mint. If we allow public access to our chain, or our network is penetrated and the blockchain is accessed, an address could mint all remaining tokens.

# openPack
```
/**
 * Open a pack and mint the moments contained within.
 * Burns the pack token, returns _moments_per_pack moments.
 * @param tokenId The token id of the pack to open and burn.
 */
function openPack(uint256 tokenId)
    public
    whenNotPaused
    returns (uint256[] memory)
{
    require(
        _momentsContract != Moments(payable(address(0))),
        "Moments contract not set."
    );
    require(_exists(tokenId), "The pack does not exist.");

    require(
        _isApprovedOrOwner(_msgSender(), tokenId),
        "Ownable: Caller is not owner or approved."
    );

    burn(tokenId);

    uint256[] memory minted_moments = new uint256[](_moments_per_pack);
    for (uint256 i = 0; i < _moments_per_pack; i++) {
        minted_moments[i] = _momentsContract.mint();
        _momentsContract.safeTransferFrom(
            address(this),
            msg.sender,
            minted_moments[i]
        );
    }

    emit PackOpened(tokenId, msg.sender);

    return minted_moments;
}
```
The meat and potatoes of the contract: opening/burning a pack and receiving Moment tokens.