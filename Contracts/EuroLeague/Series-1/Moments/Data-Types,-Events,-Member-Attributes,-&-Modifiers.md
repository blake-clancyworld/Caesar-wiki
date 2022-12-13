# Data Types
The contract uses Open Zeppelins library for Address.
```
using Address for address;
```
## Address
Collection of functions related to the address type.

# Events
None
# Modifiers
**onlyPackContract**
```
/**
 * A modifier to only allow minting if the caller is a pack contract.
 */
modifier onlyPackContract() {
    require(_packContracts[msg.sender], "Caller must be a pack contract");
    _;
}
```
This modifier requires that the mint function can only be called by a contract contained within the _packContracts mapping.

# Member Attributes
```
mapping(address => bool) internal _packContracts;
```
The member _packContracts is a mapping of address to a boolean. The address is the address of another contract, with the mapped boolean being whether this contract is eligible to be used to mint Moments from this contract.