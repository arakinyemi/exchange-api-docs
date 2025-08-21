### Query API Key of Sub-account
Applies to master accounts only.

**Rate Limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/users/subaccount/apikey`

Request Example
```
GET /api/v5/users/subaccount/apikey?subAcct=panpanBroker2
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| apiKey | String | No | API public key |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "label": "v5",
            "apiKey": "******",
            "perm": "read_only,trade",
            "ip": "1.1.1.1,2.2.2.2",
            "ts": "1597026383085"
        },
        {
            "label": "v5.1",
            "apiKey": "******",
            "perm": "read_only",
            "ip": "1.1.1.1,2.2.2.2",
            "ts": "1597026383085"
        }
    ]
}
```
