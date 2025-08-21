### Get sub-account funding balance

Query detailed balance info of Funding Account of a sub-account via the master account (applies to master accounts only).

**Rate limit**: 6 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/asset/subaccount/balances`

Request Example
```
GET /api/v5/asset/subaccount/balances?subAcct=test1
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| ccy | String | No | Single currency or multiple currencies (no more than 20) separated with comma |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
            "availBal": "37.11827078",
            "bal": "37.11827078",
            "ccy": "ETH",
            "frozenBal": "0"
        }
    ]
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency |
| bal | String | Balance |
| frozenBal | String | Frozen balance |
| availBal | String | Available balance |
