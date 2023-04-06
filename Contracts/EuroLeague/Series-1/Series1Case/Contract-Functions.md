# Constructor
```
    constructor(
        string memory name_,
        string memory symbol_,
        uint256 maxSupply,
        string memory base_uri_
    ) ClancyERC721(name_, symbol_, maxSupply, base_uri_) {}
```
No changes from ClancyERC721 constructor.

# openCase
```
    /**
     * @dev Opens a Series 1 case.
     *  safeTransferForm sends to the ownerOfToken, because using _msgSender() could send
     *  to the approved address burning the token. This is unintended behaviour.
     *
     * Requirements:
     * - The contract must not be paused.
     * - The Reels contract must be set.
     * - The caller must own the specified token.
     * - The token must exist.
     * - Burning of the token must be enabled.
     *
     * Emits a {CaseOpened} event.
     *
     * @param tokenId The ID of the token to open.
     * @return An array of the IDs of the reels that were minted.
     */
    function openCase(
        uint256 tokenId
    ) public whenNotPaused returns (uint256[] memory) {
        if (_reelsContract == Reels(payable(address(0))))
            revert ReelsContractNotSet();

        address ownerOfToken = this.ownerOf(tokenId);

        burn(tokenId);

        uint256[] memory minted_reels = new uint256[](_reelsPerCase);
        for (uint256 i = 0; i < _reelsPerCase; i++) {
            minted_reels[i] = _reelsContract.mint();
            _reelsContract.safeTransferFrom(
                address(this),
                ownerOfToken,
                minted_reels[i]
            );
        }

        emit CaseOpened(tokenId, _msgSender());

        return minted_reels;
    }
```
Opens the case and mints the set number of reels. This reels are minted to this contract, Cases, and then transferred to the owner in the same call.

# setReelsContract
```
    /**
     * @dev Sets the Reels contract instance.
     *
     * Requirements:
     * - The Reels contract address cannot be the zero address.
     * - The address provided must be a contract address.
     * - Can only be called by the owner of the contract.
     *
     * @param reelsContract The address of the Reels contract.
     */
    function setReelsContract(address reelsContract) public onlyOwner {
        if (reelsContract == address(0)) revert ReelsContractNotValid();
        if (!reelsContract.isContract()) revert ReelsContractNotValid();
        _reelsContract = Reels(payable(reelsContract));
    }
```
Sets the reels contract for the tokens to be minted when open.

# setsReelsPerCase
```
    /**
     * @dev Sets the number of reels in a case.
     *
     * Requirements:
     * - The number of reels per case must be greater than zero.
     * - Can only be called by the owner of the contract.
     *
     * @param reelsPerCase The number of reels to set per case.
     */
    function setReelsPerCase(uint8 reelsPerCase) public onlyOwner {
        if (reelsPerCase <= 0) revert ReelsPerCaseNotValid();
        _reelsPerCase = reelsPerCase;
    }
```
Sets the amount of reels to be minted for the specific case type when burned. Must be greater than 0.

# getReelsContract
```
    /**
     * @dev Returns the Reels contract.
     *
     * @return A Reels contract instance.
     */
    function getReelsContract() public view returns (Reels) {
        return _reelsContract;
    }
```
Public function for checking the reels contract address.

# getReelsPerCase
```
    /**
     * @dev Returns the number of reels in a case.
     *
     * @return A number representing the number of reels in a case.
     */
    function getReelsPerCase() public view returns (uint8) {
        return _reelsPerCase;
    }
```
Public function for getting the reels minted when the case is burned.