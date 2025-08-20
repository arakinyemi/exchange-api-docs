# Get Premium Index Price Kline
Query for historical premium index klines. Charts are returned in groups based on the requested interval.

Covers: USDT and USDC perpetual


HTTP Request
```http
GET /v5/market/premium-index-price-kline
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- |-------- | ---- | -------- |
| category | false | string | Product type. linear |
| symbol | true | string | Sy mbol name, like BTCUSDT, uppercase only |
| interval | true | string | Kline interval. 1,3,5,15,30,60,120,240,360,720,D,W,M |
| start | false | integer | The start timestamp (ms) |
| end | false | integer | The end timestamp (ms) |
| limit | false | integer | Limit for data size per page. [1, 1000]. Default: 200 |


Response Parameters 
| Parameter | Type | Comments |
| -------- | ---- | -------- |
| category | string | Product type |
| symbol | string | Symbol name |
| list | array | An string array of individual candle, Sort in reverse by start |
| > list[0] | string | Start time of the candle (ms) |
| > list[1] | string | Open price |
| > list[2] | string | Highest price |
| > list[3] | string | Lowest price |
| > list[4] | string | Close price. Is the last traded price when the candle is not closed |

---


Request Example

HTTP
```http
GET /v5/market/premium-index-price-kline?category=linear&symbol=BTCUSDT&interval=D&start=1652112000000&end=1652544000000 HTTP/1.1
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
                "1652486400000",
                "-0.000587",
                "-0.000344",
                "-0.000480",
                "-0.000344"
            ],
            [
                "1652400000000",
                "-0.000989",
                "-0.000561",
                "-0.000587",
                "-0.000587"
            ]
        ]
    },
    "retExtinfo
": {},
    "time": 1672765216291
}
```

