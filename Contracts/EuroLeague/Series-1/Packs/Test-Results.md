**Date**: 12/08/2022
Commit: 8410bc05

## Notice
Pack tests are intertwined with Moment tests. For Pack tests to be passing, all Moment tests must also be passing.
```
  Pack & Moments        
    Pack
      setMomentsContract
        √ should not set moments contract if not owner
        √ should not set the contract to the zero address
        √ should not sent the address to a non contract account
        √ Should set the moments contract
      getMomentsContract
        √ should retun the zero address if not set
        √ Should return the moments contract address
      setMomentsPerPack
        √ should not set moments per pack if not owner
        √ should not set moments per pack to zero
        √ should set moments per pack
      getMomentsPerPack
        √ default value should be 3
        √ should return the moments per pack
        √ should return 3 for both Swishn and Clutch
      mintMany
        √ should not mint if moments contract not set
        √ should not mint if minting is paused
        √ should not mint over the max_supply
        √ should mint packs
    Moment
      setPackContract
        √ should not set pack contract if not owner
        √ should not set the contract to the zero address
        √ should not sent the address to a non contract account
        √ Should set the pack contract to true
        √ should set a true contract to false
      isPackContract
        √ should retun the zero address if not set
        √ Should return the pack true for a validcontract address
    Flow of both contracts
      √ Should mint a Clutch Pack
      √ should not open a pack if minting is disabled, but burn is enabled
      √ should not open a pack if minting is enabled, but burn is 
disabled
      √ should not open a pack if minting is enabled, and burn is 
enabled, but moment minting is disabled
      √ should not open a pack for the non pack owner
      √ Should open a pack

·------------------------------------|----------------------------|-------------|-----------------------------·
|        Solc version: 0.8.17        ·  Optimizer enabled: false  
·  Runs: 200  ·  Block limit: 30000000 gas  │
·····································|····························|·············|······························
|  Methods
                                            │
·············|·······················|··············|·············|·············|···············|··············
|  Contract  ·  Method               ·  Min         ·  Max        
·  Avg        ·  # calls      ·  eth (avg)  │
·············|·······················|··············|·············|·············|···············|··············
|  Clutch    ·  mint                 ·           -  ·          -  
·     170626  ·           11  ·          -  │
·············|·······················|··············|·············|·············|···············|··············
|  Clutch    ·  mintMany             ·           -  ·          -  
·    1228860  ·            1  ·          -  │
·············|·······················|··············|·············|·············|···············|··············
|  Clutch    ·  openPack             ·           -  ·          -  
·     508348  ·            2  ·          -  │
·············|·······················|··············|·············|·············|···············|··············
|  Clutch    ·  setBurnStatus        ·       26290  ·      29102  
·      28540  ·            5  ·          -  │
·············|·······················|··············|·············|·············|···············|··············
|  Clutch    ·  setMomentsContract   ·           -  ·          -  
·      49127  ·           11  ·          -  │
·············|·······················|··············|·············|·············|···············|··············
|  Clutch    ·  setMomentsPerPack    ·           -  ·          -  
·      28989  ·            2  ·          -  │
·············|·······················|··············|·············|·············|···············|··············
|  Clutch    ·  setPublicMintStatus  ·       29096  ·      29108  
·      29107  ·            9  ·          -  │
·············|·······················|··············|·············|·············|···············|··············
|  Moments   ·  setPackContract      ·       27616  ·      49528  
·      47336  ·           10  ·          -  │
·············|·······················|··············|·············|·············|···············|··············
|  Moments   ·  setPublicMintStatus  ·       26252  ·      46164  
·      39527  ·            3  ·          -  │
·············|·······················|··············|·············|·············|···············|··············
|  Deployments                       ·
              ·  % of limit   ·             │
·····································|··············|·············|·············|···············|··············
|  Clutch                            ·     5188942  ·    5188954  
·    5188950  ·       17.3 %  ·          -  │
·····································|··············|·············|·············|···············|··············
|  Moments                           ·           -  ·          -  
·    4654711  ·       15.5 %  ·          -  │
·------------------------------------|--------------|-------------|-------------|---------------|-------------·

  29 passing (7s)
```