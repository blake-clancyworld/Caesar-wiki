**Date**: 04/06/2023

## Notice
Case tests are intertwined with Reel tests. For Reels tests to be passing, all Case tests must also be passing.
```
 Running 14 tests for test/Euroleague/Series1/Reels.t.sol:Reels_Test
[PASS] testFuzz_mint(uint256) (runs: 256, μ: 773593, ~: 347218)
[PASS] test_isCaseContract() (gas: 9999)
[PASS] test_isCaseContract_whenCaseContract() (gas: 37740)
[PASS] test_mint_1() (gas: 211001)
[PASS] test_mint_100() (gas: 12655518)
[PASS] test_mint_101() (gas: 12004283)
[PASS] test_mint_fromNonCaseContract_ShouldRevert() (gas: 38244)
[PASS] test_mint_whenPublicMintIsDisabledAndNotPaused_ShouldPass() (gas: 45484)
[PASS] test_mint_whenPublicMintIsEnabledAndPaused_ShouldRevert() (gas: 88238)
[PASS] test_setCaseContract() (gas: 37762)
[PASS] test_setCaseContractAsNonOwner_ShouldRevert() (gas: 13413)
[PASS] test_setCaseContract_AsEOA_ShouldRevert() (gas: 13509)
[PASS] test_setCaseContract_AsZeroAddress_ShouldRevert() (gas: 10852)
[PASS] test_setCaseContract_setExisingTrueToFalse_ShouldPass() (gas: 29644)
Test result: ok. 14 passed; 0 failed; finished in 95.92ms
```