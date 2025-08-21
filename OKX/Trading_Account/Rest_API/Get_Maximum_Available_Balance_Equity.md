### Get maximum available balance/equity

**Rate Limit:** 20 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

HTTP Request
```
GET /api/v5/account/max-avail-size
```

Request Example

```
# Query maximum available transaction amount for SPOT BTC-USDT
GET /api/v5/account/max-avail-size?instId=BTC-USDT&tdMode=cash
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Single instrument or multiple instruments (no more than 5) separated with comma, e.g. BTC-USDT,ETH-USDT |
| tdMode | String | Yes | Trade mode<br>cash |

Response Example

```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "instId": "BTC-USDT",
      "availBuy": "100",
      "availSell": "1"
    }
  ]
}
```

Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instId | String | Instrument ID |
| availBuy | String | Amount available to buy |
| availSell | String | Amount available to sell |

💡 In the case of SPOT, availBuy is in the quote currency, and availSell is in the base currency.
