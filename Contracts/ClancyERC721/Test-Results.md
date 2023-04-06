**Audit Id:** 586071b6-b637-42f0-94a0-3691bbad0fb0

**Date:** 02/12/2023

```
  ClancyERC721
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
    mintMany
      √ should not mint if public minting is disabled
      √ should not mint if the input is greater than _mint_many_limit
      √ should not mint if max supply reached
      √ should have an address token balance of 1
      √ should mint up to the max supply
      √ should mint 20 tokens, and have all their expected tokenURI values
    mintTo
      √ should not mint to an address for invalid owner
      √ should have the owner of the token be the calling address
      √ should not have the owner be the calling address
      √ should have a matching tokenURI with the tokenId
      √ should change the token URI
      √ should have an address token balance of 1
      √ should mint 20 tokens, and have all their expected tokenURI values
    mintToMany
      √ should not mint to an address for invalid owner
      √ should not mint to the zero address
      √ should mint to multiple addresses
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

·----------------------------------------|---------------------------|-------------|-----------------------------·
|          Solc version: 0.8.17          ·  Optimizer enabled: true  ·  Runs: 200  ·  Block limit: 30000000 gas  │
·········································|···························|·············|······························
|  Methods                                                                                                       │
·················|·······················|·············|·············|·············|···············|··············
|  Contract      ·  Method               ·  Min        ·  Max        ·  Avg        ·  # calls      ·  eth (avg)  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  burn                 ·      52330  ·      72885  ·      63725  ·            8  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  mint                 ·     154988  ·     169288  ·     164890  ·          100  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  mintMany             ·          -  ·          -  ·    2244400  ·            2  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  mintTo               ·     158679  ·     170179  ·     163558  ·           33  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  mintToMany           ·          -  ·          -  ·     292821  ·            2  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  pause                ·          -  ·          -  ·      27780  ·            7  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  renounceOwnership    ·          -  ·          -  ·      23250  ·            1  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  safeTransferFrom     ·          -  ·          -  ·      86474  ·            1  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  setApprovalForAll    ·          -  ·          -  ·      46252  ·            1  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  setBaseURI           ·          -  ·          -  ·      32724  ·            1  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  setBurnStatus        ·          -  ·          -  ·      29828  ·            8  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  setMaxSupply         ·          -  ·          -  ·      31930  ·            3  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  setPublicMintStatus  ·      29907  ·      29919  ·      29918  ·           45  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  setTokenURI          ·          -  ·          -  ·      49383  ·            2  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  transferFrom         ·          -  ·          -  ·      83650  ·            2  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  transferOwnership    ·          -  ·          -  ·      28688  ·            1  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  unpause              ·          -  ·          -  ·      27796  ·            1  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  ClancyERC721  ·  withdraw             ·      25019  ·      31719  ·      28369  ·            2  ·          -  │
·················|·······················|·············|·············|·············|···············|··············
|  Deployments                           ·                                         ·  % of limit   ·             │
·········································|·············|·············|·············|···············|··············
|  ClancyERC721                          ·          -  ·          -  ·    2907890  ·        9.7 %  ·          -  │
·----------------------------------------|-------------|-------------|-------------|---------------|-------------·

  72 passing (7s)