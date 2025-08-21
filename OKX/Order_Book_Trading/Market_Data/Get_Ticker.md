### GET / Ticker

Retrieve the latest price snapshot, best bid/ask price, and trading volume in the last 24 hours. Best ask price may be lower than the best bid price during the pre-open period.

**Rate Limit:** 20 requests per 2 seconds  
**Rate limit rule:** IP

**HTTP Request**  
`GET /api/v5/market/ticker`

#### Request Example

```
GET /api/v5/market/ticker?instId=BTC-USDT
```

#### Request Parameters

| Parameter | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| instId    | String | Yes      | Instrument ID, e.g. BTC-USDT |

#### Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instType": "SPOT",
            "instId": "BTC-USDT",
            "last": "51240",
            "lastSz": "0.49011124",
            "askPx": "51240",
            "askSz": "0.64278176",
            "bidPx": "51239.9",
            "bidSz": "1.68139044",
            "open24h": "51695.6",
            "high24h": "52080",
            "low24h": "50936",
            "volCcy24h": "539533972.680195094",
            "vol24h": "10474.12353007",
            "ts": "1708669925904",
            "sodUtc0": "51290.1",
            "sodUtc8": "51602.4"
        }
    ]
}
```

#### Response Parameters

| Parameter   | Type   | Description                                                                                      |
|-------------|--------|------------------------------------------------------------------------------------------------|
| instType    | String | Instrument type                                                                                |
| instId      | String | Instrument ID                                                                                  |
| last        | String | Last traded price                                                                             |
| lastSz      | String | Last traded size                                                                              |
| askPx       | String | Best ask price                                                                               |
| askSz       | String | Best ask size                                                                                |
| bidPx       | String | Best bid price                                                                               |
| bidSz       | String | Best bid size                                                                                |
| open24h     | String | Open price in the past 24 hours                                                             |
| high24h     | String | Highest price in the past 24 hours                                                          |
| low24h      | String | Lowest price in the past 24 hours                                                           |
| volCcy24h   | String | 24h trading volume (SPOT: quantity in quote currency)                                        |
| vol24h      | String | 24h trading volume (SPOT: quantity in base currency)                                         |
| sodUtc0     | String | Open price in the UTC 0                                                                      |
| sodUtc8     | String | Open price in the UTC 8                                                                      |
| ts          | String | Ticker data generation time, Unix timestamp in milliseconds (e.g. 1597026383085)               |

***
