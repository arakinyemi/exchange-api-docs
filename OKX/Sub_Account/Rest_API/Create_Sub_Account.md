### Create Sub-account
Applies to master accounts only and master accounts API Key must be linked to IP addresses.

**Rate Limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

HTTP Request
`POST /api/v5/users/subaccount/create-subaccount`

Request Example
```
POST /api/v5/users/subaccount/create-subaccount
body
{
    "subAcct": "subAccount002",
    "type": "1",
    "label": "123456"
}
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| type | String | Yes | Sub-account type: 1 (Standard), 5 (Custody - Copper), 12 (Custody - Komainu) |
| label | String | No | Sub-account notes (6-32 characters) |
| pwd | String | Conditional | Sub-account login password (required for KYB users) |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "label": "123456 ",
            "subAcct": "subAccount002",
            "ts": "1744875304520",
            "uid": "698827017768230914"
        }
    ]
}
```
