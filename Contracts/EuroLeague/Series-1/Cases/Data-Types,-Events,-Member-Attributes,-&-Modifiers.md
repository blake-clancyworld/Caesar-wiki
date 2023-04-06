# Raw Code
```
// SPDX-License-Identifier: None
pragma solidity ^0.8.19;

interface IReels {
    error NotCaseContract();
    error CaseContractInvalid();

    event CaseContractSet(address indexed, bool indexed);
}
```
---
## Errors
### NotCaseContract
```
error NotCaseContract();
```
Emitted when the modifier onlyCaseContract() is checked and does not match.

### CaseContractInvalid
```
error CaseContractInvalid();
```
Emitted during setCaseContract when the input address is not a contract, or is the zero address.

--------
## Events
```
event CaseContractSet(address indexed, bool indexed);
```
Emitted when the CaseContract is set.