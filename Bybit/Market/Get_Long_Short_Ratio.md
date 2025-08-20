# Get Long Short Ratio

HTTP Request
```http
GET /v5/market/account-ratio
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type. linear(USDT Contract),inverse |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| period | true | string | Data recording period. 5min, 15min, 30min, 1h, 4h, 1d |
| startTime | false | string | The start timestamp (ms) |
| endTime | false | string | The end timestamp (ms) |
| limit | false | integer | Limit for data size per page. [1, 500]. Default: 50 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > symbol | string | Symbol name |
| > buyRatio | string | The ratio of the number of long position |
| > sellRatio | string | The ratio of the number of short position |
| > timestamp | string | Timestamp (ms) |
| nextPageCursor | string | Refer to the cursor request parameter |

---


Request Example

HTTP
 
  
  
  
```http
GET /v5/market/account-ratio?category=linear&symbol=BTCUSDT&period=1h&limit=2&startTime=1696089600000&endTime=1696262400000 HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "symbol": "BTCUSDT",
                "buyRatio": "0.49",
                "sellRatio": "0.51",
                "timestamp": "1696262400000"
            },
            {
                "symbol": "BTCUSDT",
                "buyRatio": "0.4927",
                "sellRatio": "0.5073",
                "timestamp": "1696258800000"
            }
        ],
        "nextPageCursor": "lastid%3D0%26lasttime%3D1696258800"
    },
    "retExtinfo
": {},
    "time": 1731567491688
}
```

