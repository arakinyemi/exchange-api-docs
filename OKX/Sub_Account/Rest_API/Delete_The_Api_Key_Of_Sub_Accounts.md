### Delete the API Key of sub-accounts

Applies to master accounts only and master accounts API Key must be linked to IP addresses.

**Rate limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

HTTP Request
`POST /api/v5/users/subaccount/delete-apikey`

Request Example
```
POST /api/v5/users/subaccount/delete-apikey
body
{
    "subAcct":"test00001",
    "apiKey":"******"
}
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| apiKey | String | Yes | API public key |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "subAcct": "test00001"
    }]
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| subAcct | String | Sub-account name |
