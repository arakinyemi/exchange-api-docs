# Get Funding Rate History
Query for historical funding rates. Each symbol has a different funding interval. For example, if the interval is 8 hours and the current time is UTC 12, then it returns the last funding rate, which settled at UTC 8.

To query the funding rate interval, please refer to the instruments-info
 endpoint.

Covers: USDT and USDC perpetual / Inverse perpetual

info

Passing only startTime returns an error.
Passing only endTime returns 200 records up till endTime.
Passing neither returns 200 records up till the current time.

HTTP Request
```http
GET /v5/market/funding/history
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type. linear,inverse |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| startTime | false | integer | The start timestamp (ms) |
| endTime | false | integer | The end timestamp (ms) |
| limit | false | integer | Limit for data size per page. [1, 200]. Default: 200 |


Response Parameters 
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type |
| list | array | Object |
| > symbol | string | Symbol name |
| > fundingRate | string | Funding rate |
| > fundingRateTimestamp | string | Funding rate timestamp (ms) |

---


Request Example

```http
GET /v5/market/funding/history?category=linear&symbol=ETHPERP&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "category": "linear",
        "list": [
            {
                "symbol": "ETHPERP",
                "fundingRate": "0.0001",
                "fundingRateTimestamp": "1672041600000"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1672051897447
}
```

