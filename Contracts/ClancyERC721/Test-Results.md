**Audit Id:** 642ed5deb8f70000191f6b53

**Date:** 04/06/2023

```
 Running 25 tests for test/clancy/ERC/ClancyERC721.t.sol:ClancyERC721_Test
[PASS] testFuzz_SetBaseURI(string) (runs: 256, μ: 64843, ~: 72826)
[PASS] testFuzz_SetMaxSupply(uint256) (runs: 256, μ: 18917, ~: 18430)
[PASS] test_balanceOf() (gas: 179289)
[PASS] test_balanceOf_afterTransfer() (gas: 195565)
[PASS] test_baseURI() (gas: 9882)
[PASS] test_burn_AnotherAccountsToken() (gas: 192402)
[PASS] test_burn_whenBurnIsDisabled() (gas: 13033)
[PASS] test_burn_whenBurnIsEnabled() (gas: 150932)
[PASS] test_burn_whenBurnIsEnabled_andPaused() (gas: 59347)
[PASS] test_getBurnStatus() (gas: 7739)
[PASS] test_getMaxSupply() (gas: 7682)
[PASS] test_get_token_id_counter() (gas: 7694)
[PASS] test_mint_1() (gas: 178056)
[PASS] test_mint_whenPublicMintIsDisabled_andNotPaused() (gas: 10843)
[PASS] test_mint_whenPublicMintIsEnabled_andPaused() (gas: 58145)
[PASS] test_safeTransferFrom() (gas: 195589)
[PASS] test_safeTransferFrom_MintOneThensafeTransferFrom_CircuitTest() (gas: 212126)
[PASS] test_safeTransferFrom_asApprovedOrOwner() (gas: 203100)
[PASS] test_safeTransferFrom_asApprovedOrOwnerForNonApprovedToken() (gas: 331997)
[PASS] test_setBaseURI_AsNonOwner() (gas: 11449)
[PASS] test_setBurnStatus() (gas: 33031)
[PASS] test_setBurnStatus_AsNonOwner() (gas: 11392)
[PASS] test_setMaxSupply() (gas: 21054)
[PASS] test_setPublicMintStatus() (gas: 31932)
[PASS] test_setPublicMintStatus_asNonOwner() (gas: 11295)
Test result: ok. 25 passed; 0 failed; finished in 19.67ms
```