# cancelItem
Route: sentinel/marketplace/escrow/cancel
Authorization: User
Body:
```
{
    "data": {
        "token": {
            "contractName": "Clutch",
            "tokenId": 1
        }
    }
}
```

# createPurchase
Route: sentinel/marketplace/escrow/createPurchase
Authorization: bearer
Body:
```
{
    "data": {
        "buyer": "aws-username",
        "token": {
            "contractName": "Clutch",
            "tokenId": 5
        }
    }
}
```

# claim
Route: sentinel/marketplace/escrow/claim
Authorization: user
Body:
```
{
    "data": {
        "token": {
            "contractName": "Clutch",
            "tokenId": 1
        }
    }
}
```

# getItem
Route: sentinel/marketplace/escrow/getItem
Authorization: user
Body:
```
{
    "data": {
        "token": {
            "contractName": "Clutch",
            "tokenId": 1
        }
    }
}
```
# getActiveListings
Route: sentinel/marketplace/escrow/getActiveListings
Authorization: bearer
Body:
```
{
    "data": {
        "contractName": "Clutch"
    }
}
```