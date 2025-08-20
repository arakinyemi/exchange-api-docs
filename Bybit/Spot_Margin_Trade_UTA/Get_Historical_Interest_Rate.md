# Get Historical Interest Rate
You can query up to six months borrowing interest rate of Margin trading.

info

Need authentication, the api key needs "Spot" permission
Only supports Unified account
It is public data, i.e., different users get the same historical interest rate for the same VIP/Pro

HTTP Request
```http
GET /v5/spot-margin-trade/interest-rate-history
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| currency | true | string | Coin name, uppercase only |
| vipLevel | false | string | Vip level <br> Please note that "No VIP" should be passed like "No%20VIP" in the query string<br> If not passed, it returns your account's VIP level data |
| startTime | false | integer | The start timestamp (ms) <br> startTime and endTime are not passed, return 7 days by default <br> Only startTime is passed, return range between startTime and startTime+7 days <br> Only endTime is passed, return range between endTime-7 days and endTime <br> If both are passed, the rule is endTime - startTime <= 7 days |
| endTime | false | integer | The end timestamp (ms) |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array`<object>` |
| > timestamp | long | timestamp |
| > currency | string | coin name |
| > hourlyBorrowRate | string | Hourly borrowing rate |
| > vipLevel | string | VIP/Pro level |

---

Request Example

HTTP
 
```http
GET /v5/spot-margin-trade/interest-rate-history?currency=USDC&vipLevel=No%20VIP&startTime=1721458800000&endTime=1721469600000 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1721891663064
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "timestamp": 1721469600000,
                "currency": "USDC",
                "hourlyBorrowRate": "0.000014621596",
                "vipLevel": "No VIP"
            },
            {
                "timestamp": 1721466000000,
                "currency": "USDC",
                "hourlyBorrowRate": "0.000014621596",
                "vipLevel": "No VIP"
            },
            {
                "timestamp": 1721462400000,
                "currency": "USDC",
                "hourlyBorrowRate": "0.000014621596",
                "vipLevel": "No VIP"
            },
            {
                "timestamp": 1721458800000,
                "currency": "USDC",
                "hourlyBorrowRate": "0.000014621596",
                "vipLevel": "No VIP"
            }
        ]
    },
    "retExtinfo
": "{}",
    "time": 1721899048991
}
```

