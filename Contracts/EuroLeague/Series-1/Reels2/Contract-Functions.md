# setPackContract
```
/**
 * Set the contract for the packs that can mint moments.
 * @param packContract The address of the pack contract.
 * @param isValid Whether the pack contract is valid or not.
 */
function setPackContract(
    address packContract,
    bool isValid
) public onlyOwner {
    require(
        packContract != address(0),
        "Pack contract cannot be the zero address."
    );
    require(packContract.isContract(), "Address is not a contract.");
    _packContracts[packContract] = isValid;
}
```
Sets the boolean status of the contract as eligible for minting.

# isPackContract
```
 /**
  * Check whether the pack contract is valid.
  * @param packContract The address of the pack contract.
  */
 function isPackContract(address packContract) public view returns (bool) {
     return _packContracts[packContract];
 }
```
A function that returns the mapped boolean value of the private mapping.

# mint
```
/**
 * Mint a moment and assign it to the caller.
 * Overrides the base mint function as otherwise the minted moments would be assigned to the pack contract.
 */
function mint()
    public
    override
    whenNotPaused
    onlyPackContract
    returns (uint256 tokenId)
{
    return super.mint();
}
```
Overrides the BaseContract721 mint function to force usage of the onlyPackContract modifier.
- **Note**: This does not prevent the mintTo function of the BaseContract721 functionality. The owner of the contract can still mint to specific address for giveaways, rewards, etcs.