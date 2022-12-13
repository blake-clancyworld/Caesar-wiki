# Data Types
The contract uses Open Zeppelins library for Address.
using Address for address;

# Events
## PackOpened
```
event PackOpened(uint256 tokenId, address pack);
```
Emitted when a pack is opened and moments are minted.

# Member Attributes
## _momentsContract
```
Moments private _momentsContract;
```
The _momentsContract stores a reference to the Moments contract to be used for minting moments.

## _moments_per_pack
```
uint256 private _moments_per_pack = 3;
```
The quantity of moments to be minted when a pack is burned.
By default, all pack contracts will mint 3 packs - this can be raised or lowered after contract creation.

# Modifiers
None