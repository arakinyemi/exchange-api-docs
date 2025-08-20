# Get Order History
info

orderId & orderLinkId has a higher priority than startTime & endTime

Fully cancelled orders are stored for up to 24 hours.

Single leg orders can also be found with "createType"=CreateByFutureSpread via # Get Order History


HTTP Request
```http
GET /v5/spread/order/history
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| symbol | false | string | Spread combination symbol name |
| baseCoin | false | string | Base coin |
| orderId | false | string | Spread combination order ID |
| orderLinkId | false | string | User customised order ID |
| startTime | false | long | The start timestamp (ms) <br> startTime and endTime are not passed, return 7 days by default <br> Only startTime is passed, return range between startTime and startTime+7 days <br> Only endTime is passed, return range between endTime-7 days and endTime <br> If both are passed, the rule is endTime - startTime <= 7 days |
| endTime | false | long | The end timestamp (ms) |
| limit | false | integer | Limit for data size per page. [1, 50]. Default: 20 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
 
Request Example
```http
GET /v5/spread/order/history?orderId=aaaee090-fab3-42ea-aea0-c9fbfe6c4bc4 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1744100522465
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "Success",
    "result": {
        "nextPageCursor": "aaaee090-fab3-42ea-aea0-c9fbfe6c4bc4%3A1744096099767%2Caaaee090-fab3-42ea-aea0-c9fbfe6c4bc4%3A1744096099767",
        "list": [
            {
                "symbol": "SOLUSDT_SOL/USDT",
                "orderType": "Limit",
                "orderLinkId": "",
                "orderId": "aaaee090-fab3-42ea-aea0-c9fbfe6c4bc4",
                "contractType": "FundingRateArb",
                "orderStatus": "Cancelled",
                "createdAt": "1744096099767",
                "price": "-4",
                "leg2Symbol": "SOLUSDT",
                "orderQty": "0.1",
                "timeInForce": "PostOnly",
                "baseCoin": "SOL",
                "updatedAt": "1744098396079",
                "side": "Buy",
                "leg2Side": "Sell",
                "leavesQty": "0",
                "leg1Side": "Buy",
                "settleCoin": "USDT",
                "cumExecQty": "0",
                "qty": "0.1",
                "leg1OrderId": "82335b0a-b7d9-4ea5-9230-e71271a65100",
                "leg2OrderId": "1924011967786517249",
                "leg2ProdType": "Spot",
                "leg1ProdType": "Futures",
                "leg1Symbol": "SOLUSDT"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1744102655725
}
```

