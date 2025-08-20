# Get USDC Session Settlement
Query session settlement records of USDC perpetual and futures


HTTP Request
```http
GET /v5/asset/settlement-record
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type |
| UTA2.0: linear(USDC contract) |
| UTA1.0: linear(USDC contract) |
| symbol | false | string | Symbol name, like BTCPERP, uppercase only |
| startTime | false | integer | The start timestamp (ms) <br> startTime and endTime are not passed, return 30 days by default <br> Only startTime is passed, return range between startTime and startTime + 30 days <br> Only endTime is passed, return range between endTime - 30 days and endTime <br> If both are passed, the rule is endTime - startTime <= 30 days |
| endTime | false | integer | The end timestamp (ms) |
| expDate | false | string | Expiry date. 25MAR22. Default: return all |
| limit | false | integer | Limit for data size per page. [1, 50]. Default: 20 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type |
| list | array | Object |
| > symbol | string | Symbol name |
| > side | string | Buy,Sell |
| > size | string | Position size |
| > sessionAvgPrice | string | Settlement price |
| > markPrice | string | Mark price |
| > realisedPnl | string | Realised PnL |
| > createdTime | string | Created time (ms) |
| nextPageCursor | string | Refer to the cursor request parameter |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/settlement-record?category=linear HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672284883483
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "116952%3A1%2C116952%3A1",
        "category": "linear",
        "list": [
            {
                "realisedPnl": "-71.28",
                "symbol": "BTCPERP",
                "side": "Buy",
                "markPrice": "16620",
                "size": "1.5",
                "createdTime": "1672214400000",
                "sessionAvgPrice": "16620"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1672284884285
}
```

