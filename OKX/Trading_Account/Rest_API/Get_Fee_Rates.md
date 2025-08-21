### Get fee rates

**Rate Limit:** 5 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

HTTP Request
```
GET /api/v5/account/trade-fee
```

Request Example

```
# Query trade fee rate of SPOT BTC-USDT
GET /api/v5/account/trade-fee?instType=SPOT&instId=BTC-USDT
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instType | String | Yes | Instrument type<br>SPOT |
| instId | String | No | Instrument ID, e.g. BTC-USDT<br>Applicable to SPOT |

Response Example

```
{
  "code": "0",
  "msg": "",
  "data": [{
    "category": "1", //Deprecated
    "delivery": "",
    "exercise": "",
    "instType": "SPOT",
    "level": "lv1",
    "maker": "-0.0008",
    "makerU": "",
    "makerUSDC": "",
    "taker": "-0.001",
    "takerU": "",
    "takerUSDC": "",
    "ts": "1608623351857",
    "fiat": []
   }
  ]
}
```

Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| level | String | Fee rate Level |
| taker | String | For SPOT, it is taker fee rate of the USDT trading pairs. |
| maker | String | For SPOT, it is maker fee rate of the USDT trading pairs. |
| takerU | String | Taker fee rate of USDT-margined contracts, only applicable to FUTURES/SWAP |
| makerU | String | Maker fee rate of USDT-margined contracts, only applicable to FUTURES/SWAP |
| delivery | String | Delivery fee rate |
| exercise | String | Fee rate for exercising the option |
| instType | String | Instrument type |
| takerUSDC | String | For SPOT, it is taker fee rate of the USDⓈ&Crypto trading pairs. |
| makerUSDC | String | For SPOT, it is maker fee rate of the USDⓈ&Crypto trading pairs. |
| ts | String | Data return time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| category | String | Currency category. Note: this parameter is already deprecated |
| fiat | Array of objects | Details of fiat fee rate |
| > ccy | String | Fiat currency. |
| > taker | String | Taker fee rate |
| > maker | String | Maker fee rate |

💡 **Remarks:**  
The fee rate like maker and taker: positive number, which means the rate of rebate; negative number, which means the rate of commission.

💡 USDⓈ represent the stablecoin besides USDT and USDC
