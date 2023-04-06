**Date:** 04/06/2022

# Notice
Reels tests are intertwined with Series1Case tests. For Reels tests to be passing, all Series1Cases tests must also be passing.
```
 Running 13 tests for test/Euroleague/Series1/Series1Case.sol:Case_Test
[PASS] testFuzz_setReelsPerCase(uint8) (runs: 256, μ: 16748, ~: 16869)
[PASS] testFuzz_setReelsPerCaseLTEZero_ShouldRevert(uint8) (runs: 256, μ: 11589, ~: 11589)
[PASS] test_getReelsContract() (gas: 7840)
[PASS] test_getReelsPerCase() (gas: 7812)
[PASS] test_openCase() (gas: 2519776)
[PASS] test_openCase_AsApproved_ShouldSucceed() (gas: 2525300)
[PASS] test_openCase_AsNonOwner_ShouldRevert() (gas: 2196117)
[PASS] test_setReelsContract() (gas: 1982428)
[PASS] test_setReelsContractAsNonOwner_ShouldRevert() (gas: 1979491)
[PASS] test_setReelsContract_ToEOA_ShouldRevert() (gas: 13845)
[PASS] test_setReelsContract_ZeroAddress_ShouldRevert() (gas: 11132)
[PASS] test_setReelsPerCase() (gas: 13887)
[PASS] test_setReelsPerCaseAsNonOwner_ShouldRevert() (gas: 11100)
Test result: ok. 13 passed; 0 failed; finished in 23.90ms
```