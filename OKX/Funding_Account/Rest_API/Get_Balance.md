### Get balance
Retrieve the funding account balances of all the assets and the amount that is available or on hold.

**Note:** Only asset information of a currency with a balance greater than 0 will be returned.

**Rate Limit:** 6 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

HTTP Request
```
GET /api/v5/asset/balances
```

Request Example
```
GET /api/v5/asset/balances
```

Request Parameters

| Parameters | Types | Required | Description |
|------------|-------|----------|-------------|
| ccy | String | No | Single currency or multiple currencies (no more than 20) separated with comma, e.g. BTC or BTC,ETH. |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
        {
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
