# Get Closed PnL
Query user's closed profit and loss records


info

Classic account: the results are sorted by updatedTime in descending order.

UTA2.0, UTA1.0(except inverse): the results are sorted by createdTime in descending order, this will be constant with classic account afterwards

UTA2.0, UTA1.0(except inverse): support getting the past 730 days historical data


HTTP Request
```http
GET /v5/position/closed-pnl
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> UTA2.0, UTA1.0: linear(USDT Contract, USDC Contract), inverse, option<br> Classic account: linear(USDT Perps), inverse |
| symbol | false | string | Symbol name, like BTCUSDT, uppercase only |
| startTime | false | integer | The start timestamp (ms) <br> startTime and endTime are not passed, return 7 days by default <br> Only startTime is passed, return range between startTime and startTime+7 days <br> Only endTime is passed, return range between endTime-7 days and endTime<br>If both are passed, the rule is endTime - startTime <= 7 days |
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
| > side | string | Buy, Sell |
| > qty | string | Order qty |
| > orderPrice | string | Order price |
| > orderType | string | Order type. Market,Limit |
| > execType | string | Exec type <br> Trade, BustTrade <br> SessionSettlePnL <br> Settle, MovePosition |
| > closedSize | string | Closed size |
| > cumEntryValue | string | Cumulated Position value |
| > avgEntryPrice | string | Average entry price |
| > cumExitValue | string | Cumulated exit position value |
| > avgExitPrice | string | Average exit price |
| > closedPnl | string | Closed PnL |
| > fillCount | string | The number of fills in a single order |
| > leverage | string | leverage |
| > openFee | string | Open position trading fee |
| > closeFee | string | Close position trading fee |
| > createdTime | string | The created time (ms) |
| > updatedTime | string | The updated time (ms) |
| nextPageCursor | string | Refer to the cursor request parameter |

---


Request Example

HTTP
 
  
  
```http
GET /v5/position/closed-pnl?category=linear&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672284128523
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "5a373bfe-188d-4913-9c81-d57ab5be8068%3A1672214887231423699%2C5a373bfe-188d-4913-9c81-d57ab5be8068%3A1672214887231423699",
        "category": "linear",
        "list": [
            {
                "symbol": "ETHPERP",
                "orderType": "Market",
                "leverage": "3",
                "updatedTime": "1672214887236",
                "side": "Sell",
                "orderId": "5a373bfe-188d-4913-9c81-d57ab5be8068",
                "closedPnl": "-47.4065323",
                "avgEntryPrice": "1194.97516667",
                "qty": "3",
                "cumEntryValue": "3584.9255",
                "createdTime": "1672214887231",
                "orderPrice": "1122.95",
                "closedSize": "3",
                "avgExitPrice": "1180.59833333",
                "execType": "Trade",
                "fillCount": "4",
                "cumExitValue": "3541.795"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1672284129153
}
```

