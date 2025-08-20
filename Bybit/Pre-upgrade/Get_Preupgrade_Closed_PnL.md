# Get Pre-upgrade Closed PnL
Query user's closed profit and loss records from before you upgraded the account to a Unified account. The results are sorted by updatedTime in descending order.

it only supports to query USDT perpetual, Inverse perpetual and Inverse Futures.

UTA2.0:
By category=linear, you can query USDT Perps data occurred during classic account
By category=inverse, you can query Inverse Contract data occurred during classic account or UTA1.0

UTA1.0:
By category=linear, you can query USDT Perps data occurred during classic account


HTTP Request
```http
GET /v5/pre-upgrade/position/closed-pnl
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type linear, inverse |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| startTime | false | integer | The start timestamp (ms) <br> startTime and endTime are not passed, return 7 days by default <br> Only startTime is passed, return range between startTime and startTime+7 days <br> Only endTime is passed, return range between endTime-7 days and endTime <br> If both are passed, the rule is endTime - startTime <= 7 days |
| endTime | false | integer | The end timestamp (ms) |
| limit | false | integer | Limit for data size per page. [1, 100]. Default: 50 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type |
| list | array | Object |
| > symbol | string | Symbol name |
| > orderId | string | Order ID |
| > side | string | Buy, Side |
| > qty | string | Order qty |
| > orderPrice | string | Order price |
| > orderType | string | Order type. Market,Limit |
| > execType | string | Exec type. Trade, BustTrade, SessionSettlePnL, Settle |
| > closedSize | string | Closed size |
| > cumEntryValue | string | Cumulated Position value |
| > avgEntryPrice | string | Average entry price |
| > cumExitValue | string | Cumulated exit position value |
| > avgExitPrice | string | Average exit price |
| > closedPnl | string | Closed PnL |
| > fillCount | string | The number of fills in a single order |
| > leverage | string | leverage |
| > createdTime | string | The created time (ms) |
| > updatedTime | string | The updated time (ms) |
| nextPageCursor | string | Refer to the cursor request parameter |

---

Request Example
```http
GET /v5/pre-upgrade/position/closed-pnl?category=linear&symbol=BTCUSDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1682580911998
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
                "symbol": "BTCUSDT",
                "orderId": "67836246-460e-4c52-a009-af0c3e1d12bc",
                "side": "Sell",
                "qty": "0.200",
                "orderPrice": "27203.40",
                "orderType": "Market",
                "execType": "Trade",
                "closedSize": "0.200",
                "cumEntryValue": "5588.88",
                "avgEntryPrice": "27944.40",
                "cumExitValue": "5726.4252",
                "avgExitPrice": "28632.13",
                "closedPnl": "204.25510011",
                "fillCount": "22",
                "leverage": "10",
                "createdTime": "1682487465732",
                "updatedTime": "1682487465732"
            }
        ],
        "category": "linear",
        "nextPageCursor": ""
    },
    "retExtinfo
": {},
    "time": 1682580912259
}
```

