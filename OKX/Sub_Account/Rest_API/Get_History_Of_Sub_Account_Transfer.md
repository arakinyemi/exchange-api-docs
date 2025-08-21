### Get history of sub-account transfer

This endpoint is only available for master accounts. Transfer records are available from September 28, 2022 onwards.

**Rate limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/asset/subaccount/bills`

Request Example
```
GET /api/v5/asset/subaccount/bills
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | No | Currency |
| type | String | No | Transfer type (0: Master to sub-account, 1: Sub-account to master) |
| subAcct | String | No | Sub-account name |
| after | String | No | Query data earlier than timestamp (Unix ms) |
| before | String | No | Query data newer than timestamp (Unix ms) |
| limit | String | No | Number of results (max 100, default 100) |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
      {
        "amt": "1.1",
        "billId": "89887685",
        "ccy": "USDT", 
        "subAcct": "hahatest1",
        "ts": "1712560959000",
        "type": "0"
      }
    ]
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| billId | String | Bill ID |
| ccy | String | Transfer currency |
| amt | String | Transfer amount |
| type | String | Bill type |
| subAcct | String | Sub-account name |
| ts | String | Bill ID creation time (Unix ms) |
