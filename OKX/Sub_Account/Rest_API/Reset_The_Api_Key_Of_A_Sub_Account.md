### Reset the API Key of a sub-account

Applies to master accounts only and master accounts API Key must be linked to IP addresses.

**Rate limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

HTTP Request
`POST /api/v5/users/subaccount/modify-apikey`

Request Example
```
POST /api/v5/users/subaccount/modify-apikey
body
{
    "subAcct":"yongxu",
    "apiKey":"******",
    "ip":"1.1.1.1"
}
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| apiKey | String | Yes | Sub-account APIKey |
| label | String | No | Sub-account API Key label. The label will be reset if this is passed through. |
| perm | String | No | Sub-account API Key permissions (read_only: Read, trade: Trade). Separate with commas if more than one. The permission will be reset if this is passed through. |
| ip | String | No | Sub-account API Key linked IP addresses, separate with commas if more than one. Support up to 20 IP addresses. The IP will be reset if this is passed through. If ip is set to "", then no IP addresses is linked to the APIKey. |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "subAcct": "yongxu",
        "label": "v5",
        "apiKey": "******",
        "perm": "read,trade",
        "ip": "1.1.1.1",
        "ts": "1597026383085"
    }]
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| subAcct | String | Sub-account name |
| apiKey | String | Sub-account API public key |
| label | String | Sub-account API Key label |
| perm | String | Sub-account API Key permissions (read_only: Read, trade: Trade) |
| ip | String | Sub-account API Key IP addresses that linked with API Key |
| ts | String | Creation time |
