# Constructor
```
  constructor(
        string memory name_,
        string memory symbol_,
        uint256 max_supply_,
        string memory base_uri_
    ) ClancyERC721(name_, symbol_, max_supply_, base_uri_) {}
```
No special additions to ClancyERC721.

# Functions
## setCaseContract
```
    /**
     * @dev Sets the validity of a Case contract.
     *
     * Requirements:
     * - The Case contract address cannot be the zero address.
     * - The address provided must be a contract address.
     * - Can only be called by the owner of the contract.
     *
     * @param caseContract The address of the Case contract to set.
     * @param isValid A boolean value indicating the validity of the Case contract.
     */
    function setCaseContract(
        address caseContract,
        bool isValid
    ) public onlyOwner {
        if (caseContract == address(0)) revert CaseContractInvalid();
        if (!caseContract.isContract()) revert CaseContractInvalid();
        _caseContracts[caseContract] = isValid;
        emit CaseContractSet(caseContract, isValid);
    }
```
Sets the case contract.
Allows multiple case contracts i.e. HeatinUp, Swishin, etc.

## isCaseContract
```
    /**
     * @dev Checks if an address is a Case contract.
     *
     * @param caseContract The address to check.
     * @return True if the address is a Case contract, false otherwise.
     */
    function isCaseContract(address caseContract) public view returns (bool) {
        return _caseContracts[caseContract];
    }
```
Public function for checking whether an address is a valid case contract.

## mint
```
    /**
     * @dev Mints a new Series 1 case.
     *
     * Requirements:
     * - The contract must not be paused.
     * - The function can only be called by the Case contract.
     * - Is NOT user callable. This is for owner intervention.
     * @return The ID of the token that was minted.
     */
    function mint() public override whenNotPaused onlyOwner returns (uint256) {
        return super.mint();    
    }
```
Overrides ClancyERC721 base minting function.
### Notes
- This overrides the default mint functionality and prevents it from public use. Only the owner can use this i.e. airdrops, rewards.

## mintTo
```
    /**
     * @notice Mints a new ERC721 token to the specified `to` address
     * @dev Can only be called when contract is not paused and by the case contract
     * @param to The address to mint the token to
     * @return tokenId The ID of the newly minted token
     */
    function mintTo(
        address to
    ) public whenNotPaused onlyCaseContract returns (uint256) {
        return clancyMint(to);
    }
```
The main function used for opening cases. Required for use of onlyCaseContract modifier.