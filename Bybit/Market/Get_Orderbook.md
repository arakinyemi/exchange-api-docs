# Get Orderbook
Query spread orderbook depth data.


HTTP Request
```http
GET /v5/market/orderbook
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| symbol | true | string | Spread combination symbol name |
| limit | false | integer | Limit size for each bid and ask [1, 25]. Default: 1 |


Response Parameters 
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| s | string | Spread combination symbol name |
| b | array | Bid, buyer. Sorted by price in descending order |
| > b[0] | string | Bid price |
| > b[1] | string | Bid size |
| a | array | Ask, seller. Sorted by price in ascending order |
| > a[0] | string | Ask price |
| > a[1] | string | Ask size |
| ts | integer | The timestamp (ms) that the system generates the data |
| u | integer | Update ID. Is always in sequence. Corresponds to u in the 25-level WebSocket orderbook stream |
| seq | integer | Cross sequence |
| cts | integer | The timestamp from the matching engine when this orderbook data is produced. It can be correlated with T from public trade channel |

---

Request Example
```http
GET /v5/market/orderbook?category=spot&symbol=BTCUSDT HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "s": "BTCUSDT",
        "a": [
            [
                "65557.7",
                "16.606555"
            ]
        ],
        "b": [
            [
                "65485.47",
                "47.081829"
            ]
        ],
        "ts": 1716863719031,
        "u": 230704,
        "seq": 1432604333,
        "cts": 1716863718905
    },
    "retExtinfo
": {},
    "time": 1716863719382
}
```

