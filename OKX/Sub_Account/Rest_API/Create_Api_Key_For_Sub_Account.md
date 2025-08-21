### Create API Key for Sub-account
Applies to master accounts only.

**Rate Limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

HTTP Request
`POST /api/v5/users/subaccount/apikey`

Request Example
```
POST /api/v5/users/subaccount/apikey
body
{
    "subAcct":"panpanBroker2",
    "label":"broker3",
    "passphrase": "******",
    "perm":"trade"
}
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name (6-20 alphanumeric chars) |
| label | String | Yes | API Key note |
| passphrase | String | Yes | API Key password (8-32 chars with complexity) |
| perm | String | No | API Key permissions: read_only or trade |
| ip | String | No | Linked IP addresses (max 20) |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "subAcct": "test-1",
        "label": "v5",
        "apiKey": "******",
        "secretKey": "******",
        "passphrase": "******",
        "perm": "read_only,trade",
        "ip": "1.1.1.1,2.2.2.2",
        "ts": "1597026383085"
    }]
}
```
