**Git Commit:** [8410bc05](https://spsprodcca1.vssps.visualstudio.com/_signin?realm=dev.azure.com&reply_to=https%3A%2F%2Fdev.azure.com%2Fclancyworld%2FCaesar%2F_git%2Fblockchain-contracts%2Fcommit%2F8410bc05ae98985df8a77b2bd0a389f0a117d060%3FrefName%3Drefs%252Fheads%252Fdevelop%252Fv0.1.0&redirect=1&mkt=en-CA&hid=95287c9d-8e35-4824-81c4-d847f45fdd8f&context=eyJodCI6MiwiaGlkIjoiZDc3NTVjNDktYmNmNC00MTk2LTg0OWQtZmI4YWMyYTg0MGU3IiwicXMiOnt9LCJyciI6IiIsInZoIjoiIiwiY3YiOiIiLCJjcyI6IiJ90#ctx=eyJTaWduSW5Db29raWVEb21haW5zIjpbImh0dHBzOi8vbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbSIsImh0dHBzOi8vbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbSJdfQ2)

**Audit Id:** 586071b6-b637-42f0-94a0-3691bbad0fb0

**Date:** 12/02/2022

```
  BaseContract721
    constructor
      √ Contract name should be as specified
      √ Contract symbol should be as specified
      √ Contract base URI should be as specified
      √ Contract max supply should be as specified
    Renounce
      √ Should renounce for valid owner
      √ Should not renounce for invalid owner
    setBaseURI
      √ Should not set base URI for invalid owner
      √ Should set base URI for valid owner
    withdraw
      √ Should not withdraw for invalid owner
      √ Should withdraw for valid owner
      √ should send 1 eth to the contract, and not withdraw
      √ should send 1 eth to the contract, and withdraw
    max supply
      √ should not change the max supply to less than the current supply
      √ should not change the max supply to less than 1
      √ should change the max supply
      √ should get the max supply
    totalSupply
      √ should get the total supply
      √ should get the total supply after minting
      √ should get the total supply after minting multiple
      √ should get the total supply after minting and burning
    mint
      √ should not mint if public minting is disabled
      √ should not mint if max supply reached
      √ should have the owner of the token be the calling address
      √ should have the owner be the calling address
      √ should have a matching tokenURI with the tokenId
      √ should change the token URI
      √ should have an address token balance of 1
      √ should mint 20 tokens, and have all their expected tokenURI values
    mintTo
      √ should not mint to an address for invalid owner
      √ should have the owner of the token be the calling address
      √ should not have the owner be the calling address
      √ should have a matching tokenURI with the tokenId
      √ should change the token URI
      √ should have an address token balance of 1
      √ should mint 20 tokens, and have all their expected tokenURI values
    _token_id_counter
      √ should start at 0
      √ should increment by 1 when a token is minted
      √ should decrement by 1 when a token is burned
      Live Scenario
        √ should mint 5 tokens and have the token id counter be 5
        √ should burn 2 tokens and have the token id counter 5, but the totalSupply be 3
        √ should mint 2 more tokens, and have a token id counter of 7, and a totalSupply of 5
    burn
      √ should not set the burn status to true if called by an invalid owner
      √ should set the burn status to true
      √ should not burn a token for an invalid owner
      √ should add the contract owner as the owner of the token, and the contract owner should be able to burn the token
      √ should burn a token for valid owner
    pause and unpause
      √ should not pause if called by an invalid owner
      √ should pause
      √ should not be able to mint publiclly when paused
      √ should not be able to transfer a token when paused
      √ should not unpause if called by an invalid owner
      √ should unpause
    balanceOf
      √ should have a balance of 0
      √ should have a balance of 1
      √ should have a balance of 1, transfer the token to another address, and have a balance of 0
    transferFrom
      √ should not transfer a token if called by an invalid owner
      √ should not transfer if paused
      √ should transfer a token
    safeTransferFrom
      √ should not transfer a token if called by an invalid owner
      √ should not transfer if paused
      √ should transfer a token
    transferOwnership
      √ should not transfer ownership if called by an invalid owner
      √ should transfer ownership

·-------------------------------------------|----------------------------|-------------|-----------------------------·
|           Solc version: 0.8.17            ·  Optimizer enabled: false  ·  Runs: 200  ·  Block limit: 30000000 gas  │
············································|····························|·············|······························
|  Methods                                                                                                           │
····················|·······················|··············|·············|·············|···············|··············
|  Contract         ·  Method               ·  Min         ·  Max        ·  Avg        ·  # calls      ·  eth (avg)  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  burn                 ·       53398  ·      74082  ·      64882  ·            8  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  mint                 ·      156304  ·     170604  ·     165606  ·           88  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  mintTo               ·      160082  ·     171582  ·     164961  ·           33  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  pause                ·           -  ·          -  ·      28241  ·            7  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  renounceOwnership    ·           -  ·          -  ·      23507  ·            1  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  safeTransferFrom     ·           -  ·          -  ·      87796  ·            1  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  setApprovalForAll    ·           -  ·          -  ·      46693  ·            1  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  setBaseURI           ·           -  ·          -  ·      30202  ·            1  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  setBurnStatus        ·           -  ·          -  ·      28992  ·            8  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  setMaxSupply         ·           -  ·          -  ·      32461  ·            2  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  setPublicMintStatus  ·       24296  ·      46208  ·      45582  ·           35  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  setTokenURI          ·           -  ·          -  ·      50347  ·            2  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  transferFrom         ·           -  ·          -  ·      84825  ·            2  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  transferOwnership    ·           -  ·          -  ·      29074  ·            1  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  unpause              ·           -  ·          -  ·      28261  ·            1  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  BaseContract721  ·  withdraw             ·       23780  ·      30480  ·      27130  ·            2  ·          -  │
····················|·······················|··············|·············|·············|···············|··············
|  Deployments                              ·                                          ·  % of limit   ·             │
············································|··············|·············|·············|···············|··············
|  BaseContract721                          ·           -  ·          -  ·    4431765  ·       14.8 %  ·          -  │
·-------------------------------------------|--------------|-------------|-------------|---------------|-------------·

```
  63 passing (7s)