```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

import "@openzeppelin/contracts/utils/Counters.sol";
import "../../Templates/Base/BaseContract721.sol";

contract BaseBetaPass is BaseContract721 {
    constructor(
        string memory name_,
        string memory symbol_,
        string memory baseURI_,
        uint256 max_supply_
    ) BaseContract721(name_, symbol_, baseURI_, max_supply_) {}
}
```