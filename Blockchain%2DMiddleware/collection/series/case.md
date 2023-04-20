# mint
Route: sentinel/collection/series1/series1case/mint
Authorization: bearer
Body:
```
{
    "data": {
        "contractName": "Clutch",
        "aws_username": "aws-username"
    }
}
```
# open
Route: sentinel/collection/series1/series1case/open
Authorization: user
Body:
```
{
    "data": {
        "token": {
            "contractName": "Clutch",
            "tokenId": 3
        }
    }
}
```