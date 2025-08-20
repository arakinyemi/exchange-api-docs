# Get Mark Price Kline
Query for historical mark price klines. Charts are returned in groups based on the requested interval.

Covers: USDT contract / USDC contract / Inverse contract


HTTP Request
```http
GET /v5/market/mark-price-kline
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | false | string | Product type. linear,inverse When category is not passed, use linear by default |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| interval | true | string | Kline interval. 1,3,5,15,30,60,120,240,360,720,D,M,W |
| start | false | integer | The start timestamp (ms) |
| end | false | integer | The end timestamp (ms) |
| limit | false | integer | Limit for data size per page. [1, 1000]. Default: 200 |


Response Parameters 
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type |
| symbol | string | Symbol name |
| list | array | An string array of individual candle. Sort in reverse by startTime |
| > list[0]: startTime | string | Start time of the candle (ms) |
| > list[1]: openPrice | string | Open price |
| > list[2]: highPrice | string | Highest price |
| > list[3]: lowPrice | string | Lowest price |
| > list[4]: closePrice | string | Close price. Is the last traded price when the candle is not closed |

---


Request Example

HTTP
```http
GET /v5/market/mark-price-kline?category=linear&symbol=BTCUSDT&interval=15&start=1670601600000&end=1670608800000&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "symbol": "BTCUSDT",
        "category": "linear",
        "list": [
            [
            "1670608800000",
            "17164.16",
            "17164.16",
            "17121.5",
            "17131.64"
            ]
        ]
    },
    "retExtinfo
": {},
    "time": 1672026361839
}
```

