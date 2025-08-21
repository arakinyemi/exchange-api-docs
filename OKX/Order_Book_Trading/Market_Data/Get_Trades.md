### GET / Trades

Retrieve the recent transactions of an instrument.

**Rate Limit:** 100 requests per 2 seconds  
**Rate limit rule:** IP

**HTTP Request**  
`GET /api/v5/market/trades`

#### Request Example

```
GET /api/v5/market/trades?instId=BTC-USDT
```

#### Request Parameters

| Parameter | Type   | Required | Description                          |
| --------- | ------ | -------- | ---------------------------------- |
| instId    | String | Yes      | Instrument ID, e.g. BTC-USDT         |
| limit     | String | No       | Number of results per request (max 500, default 100) |

#### Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instId": "BTC-USDT",
            "side": "sell",
            "sz": "0.00001",
            "px": "29963.2",
            "tradeId": "242720720",
            "ts": "1654161646974"
        },
        {
            "instId": "BTC-USDT",
            "side": "sell",
            "sz": "0.00001",
            "px": "29964.1",
            "tradeId": "242720719",
            "ts": "1654161641568"
        }
    ]
}
```

#### Response Parameters

| Parameter | Type   | Description                                                     |
|-----------|--------|-----------------------------------------------------------------|
| instId    | String | Instrument ID                                                  |
| tradeId   | String | Trade ID                                                      |
| px        | String | Trade price                                                  |
| sz        | String | Trade quantity (Spot unit: base currency, FUTURES/SWAP/OPTION: contracts) |
| side      | String | Trade side of taker (buy or sell)                            |
| ts        | String | Trade time, Unix timestamp in milliseconds                    |

Up to 500 most recent historical public transaction data can be retrieved.

***
