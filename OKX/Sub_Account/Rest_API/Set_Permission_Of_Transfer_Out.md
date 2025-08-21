### Set permission of transfer out

Set permission of transfer out for sub-account (only applicable to master account API key). Sub-account can transfer out to master account by default.

**Rate Limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

HTTP Request
`POST /api/v5/users/subaccount/set-transfer-out`

Request Example
```
POST /api/v5/users/subaccount/set-transfer-out
body
{
    "subAcct": "Test001,Test002",
    "canTransOut": true
}
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name(s), comma-separated (max 20) |
| canTransOut | Boolean | No | Transfer out permission (default true) |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "subAcct": "Test001",
            "canTransOut": true
        },
        {
            "subAcct": "Test002",
            "canTransOut": true
        }
    ]
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| subAcct | String | Sub-account name |
| canTransOut | Boolean | Transfer out permission |
