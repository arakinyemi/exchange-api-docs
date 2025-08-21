### Get Convert Currency Pair

**Rate Limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/asset/convert/currency-pair`

Request Example
```
GET /api/v5/asset/convert/currency-pair?fromCcy=USDT&toCcy=BTC
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| fromCcy | String | Yes | Currency to convert from, e.g. USDT |
| toCcy | String | Yes | Currency to convert to, e.g. BTC |

Response Example
```
{
    "code": "0",
    "data": [
        {
            "baseCcy": "BTC",
            "baseCcyMax": "0.5",
            "baseCcyMin": "0.0001",
            "instId": "BTC-USDT",
            "quoteCcy": "USDT",
            "quoteCcyMax": "10000",
            "quoteCcyMin": "1"
        }
    ],
    "msg": ""
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| instId | String | Currency pair, e.g. BTC-USDT |
| baseCcy | String | Base currency, e.g. BTC in BTC-USDT |
| baseCcyMax | String | Maximum amount of base currency |
| baseCcyMin | String | Minimum amount of base currency |
| quoteCcy | String | Quote currency, e.g. USDT in BTC-USDT |
| quoteCcyMax | String | Maximum amount of quote currency |
| quoteCcyMin | String | Minimum amount of quote currency |
