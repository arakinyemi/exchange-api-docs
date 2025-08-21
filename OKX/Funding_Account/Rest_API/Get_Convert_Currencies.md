### Get Convert Currencies

**Rate Limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/asset/convert/currencies`

Request Example
```
GET /api/v5/asset/convert/currencies
```

Response Example
```json
{
    "code": "0",
    "data": [
        {
            "min": "",  // Deprecated
            "max": "",  // Deprecated
            "ccy": "BTC"
        },
        {
            "min": "",
            "max": "",
            "ccy": "ETH"
        }
    ],
    "msg": ""
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency, e.g. BTC |
| min | String | Minimum amount to convert (Deprecated) |
| max | String | Maximum amount to convert (Deprecated) |
