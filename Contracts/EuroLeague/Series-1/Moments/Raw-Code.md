# Moments.sol
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import "../../Templates/Base/BaseContract721.sol";
import "@openzeppelin/contracts/utils/Address.sol";

contract Moments is BaseContract721 {
    using Address for address;
    mapping(address => bool) internal _packContracts;

    constructor(
        string memory name_,
        string memory symbol_,
        string memory baseURI_,
        uint256 max_supply_
    ) BaseContract721(name_, symbol_, baseURI_, max_supply_) {}

    /**
     * A modifier to only allow minting if the caller is a pack contract.
     */
    modifier onlyPackContract() {
        require(_packContracts[msg.sender], "Caller must be a pack contract");
        _;
    }

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

    /**
     * Check whether the pack contract is valid.
     * @param packContract The address of the pack contract.
     */
    function isPackContract(address packContract) public view returns (bool) {
        return _packContracts[packContract];
    }

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
}
```