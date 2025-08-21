### Get Sub-account List

Applies to master accounts only.

**Rate Limit**: 2 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/users/subaccount/list`

Request Example
```
GET /api/v5/users/subaccount/list
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| enable | String | No | Sub-account status: true (Normal), false (Frozen) |
| subAcct | String | No | Sub-account name |
| after | String | No | Query data earlier than requested timestamp (Unix ms) |
| before | String | No | Query data newer than requested timestamp (Unix ms) |
| limit | String | No | Number of results (max 100, default 100) |

Response Example
```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "canTransOut": false,
            "enable": true,
            "frozenFunc": [],
            "gAuth": false,
            "label": "D456DDDLx",
            "mobile": "",
            "subAcct": "D456DDDL",
            "ts": "1659334756000",
            "type": "1",
            "uid": "3400***********7413",
            "subAcctLv": "1",
            "firstLvSubAcct": "D456DDDL",
            "ifDma": false
        }
    ]
}
```
