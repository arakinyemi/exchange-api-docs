# Get Open Interest

Get the open interest of each symbol.

Covers: USDT contract / USDC contract / Inverse contract

info

The upper limit time you can query is the launch time of the symbol.

HTTP Request
```http
GET /v5/market/open-interest
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type. linear,inverse |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| intervalTime | true | string | Interval time. 5min,15min,30min,1h,4h,1d |
| startTime | false | integer | The start timestamp (ms) |
| endTime | false | integer | The end timestamp (ms) |
| limit | false | integer | Limit for data size per page. [1, 200]. Default: 50 |
| cursor | false | string | Cursor. Used to paginate |

Response Parameters
| Parameter | Type | Comments |
| -------- | ---- | -------- |
| category | string | Product type |
| symbol | string | Symbol name |
| list | array | Object |
| > openInterest | string | Open interest. The value is the sum of both sides. The unit of value, e.g., BTCUSD(inverse) is USD, BTCUSDT(linear) is BTC |
| > timestamp | string | The timestamp (ms) |
| nextPageCursor | string | Used to paginate |

---


Request Example

HTTP
```http
GET /v5/market/open-interest?category=inverse&symbol=BTCUSD&intervalTime=5min&startTime=1669571100000&endTime=1669571400000 HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "symbol": "BTCUSD",
        "category": "inverse",
        "list": [
            {
                "openInterest": "461134384.00000000",
                "timestamp": "1669571400000"
            },
            {
                "openInterest": "461134292.00000000",
                "timestamp": "1669571100000"
            }
        ],
        "nextPageCursor": ""
    },
    "retExtinfo
": {},
    "time": 1672053548579
}
```

