# Get Open & Closed Orders
Primarily query unfilled or partially filled orders in real-time, but also supports querying recent 500 closed status (Cancelled, Filled) orders. Please see the usage of request param openOnly.
And to query older order records, please use the order history interface.


tip


UTA2.0 can query filled, cancelled, and rejected orders to the most recent 500 orders for spot, linear, inverse and option categories

UTA1.0 can query filled, cancelled, and rejected orders to the most recent 500 orders for spot, linear, and option categories. The inverse category is not subject to this limitation.

You can query by symbol, baseCoin, orderId and orderLinkId, and if you pass mul
tip
le params, the system will process them according to this priority: orderId > orderLinkId > symbol > baseCoin.

The records are sorted by the createdTime from newest to oldest.

info


classic account spot can return open orders only
After a server release or restart, filled, cancelled, and rejected orders of Unified account should only be queried through order history.


HTTP Request
```http
GET /v5/order/realtime
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> UTA2.0, UTA1.0: linear, inverse, spot, option <br> classic account: linear, inverse, spot |
| symbol | false | string | Symbol name, like BTCUSDT, uppercase only. For linear, either symbol, baseCoin, settleCoin is required |
| baseCoin | false | string | Base coin, uppercase only <br> Supports linear, inverse & option <br> option: it returns all option open orders by default |
| settleCoin | false | string | Settle coin, uppercase only <br> linear: either symbol, baseCoin or settleCoin is required<br> spot: not supported <br> option: USDT or USDC |
| orderId | false | string | Order ID |
| orderLinkId | false | string | User customised order ID |
| openOnly | false | integer | 0(default): UTA2.0, UTA1.0, classic account query open status orders (e.g., New, PartiallyFilled) only <br> 1: UTA2.0, UTA1.0(except inverse) <br> 2: UTA1.0(inverse), classic account <br> Query a maximum of recent 500 closed status records are kept under each account each category (e.g., Cancelled, Rejected, Filled orders).<br> If the Bybit service is restarted due to an update, this part of the data will be cleared and accumulated again, but the order records will still be queried in order history <br> openOnly param will be ignored when query by orderId or orderLinkId <br> Classic spot: not supported |
| orderFilter | false | string | Order: active order, StopOrder: conditional order for Futures and Spot, tpslOrder: spot TP/SL order, OcoOrder: Spot oco order, BidirectionalTpslOrder: Spot bidirectional TPSL order <br> classic account spot: return Order active order by default <br> Others: all kinds of orders by default |
| limit | false | integer | Limit for data size per page. [1, 50]. Default: 20 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type |
| nextPageCursor | string | Refer to the cursor request parameter |
| list | array | Object |
| > orderId | string | Order ID |
| > orderLinkId | string | User customised order ID |
| > blockTradeId | string | Paradigm block trade ID |
| > symbol | string | Symbol name |
| > price | string | Order price |
| > qty | string | Order qty |
| > side | string | Side. Buy,Sell |
| > isLeverage | string | Whether to borrow. Unified spot only. 0: false, 1: true. Classic spot is not supported, always 0 |
| > positionIdx | integer | Position index. Used to identify positions in different position modes. |
| > orderStatus | string | Order status |
| > createType | string | Order create type <br> Only for category=linear or inverse <br> Spot, Option do not have this key |
| > cancelType | string | Cancel type |
| > rejectReason | string | Reject reason. Classic spot is not supported |
| > avgPrice | string | Average filled price <br> UTA: returns "" for those orders without avg price <br> classic account: returns "0" for those orders without avg price, and also for those orders have partilly filled but cancelled at the end |
| > leavesQty | string | The remaining qty not executed. Classic spot is not supported |
| > leavesValue | string | The estimated value not executed. Classic spot is not supported |
| > cumExecQty | string | Cumulative executed order qty |
| > cumExecValue | string | Cumulative executed order value. Classic spot is not supported |
| > cumExecFee | string | Cumulative executed trading fee. Classic spot is not supported |
| > timeinfo
rce | string | Time in force |
| > orderType | string | Order type. Market,Limit. For TP/SL order, it means the order type after triggered |
| > stopOrderType | string | Stop order type |
| > orderIv | string | Implied volatility |
| > marketUnit | string | The unit for qty when create Spot market orders for UTA account. baseCoin, quoteCoin |
| > triggerPrice | string | Trigger price. If stopOrderType=TrailingStop, it is activate price. Otherwise, it is trigger price |
| > takeProfit | string | Take profit price |
| > stopLoss | string | Stop loss price |
| > tpslMode | string | TP/SL mode, Full: entire position for TP/SL. Partial: partial position tp/sl. Spot does not have this field, and Option returns always "" |
| > ocoTriggerBy | string | The trigger type of Spot OCO order.OcoTriggerByUnknown, OcoTriggerByTp, OcoTriggerByBySl. Classic spot is not supported |
| > tpLimitPrice | string | The limit order price when take profit price is triggered |
| > slLimitPrice | string | The limit order price when stop loss price is triggered |
| > tpTriggerBy | string | The price type to trigger take profit |
| > slTriggerBy | string | The price type to trigger stop loss |
| > triggerDirection | integer | Trigger direction. 1: rise, 2: fall |
| > triggerBy | string | The price type of trigger price |
| > lastPriceOnCreated | string | Last price when place the order, Spot is not applicable |
| > basePrice | string | Last price when place the order, Spot has this field only |
| > reduceOnly | boolean | Reduce only. true means reduce position size |
| > closeOnTrigger | boolean | Close on trigger. What is a close on trigger order? |
| > placeType | string | Place type, option used. iv, price |
| > smpType | string | SMP execution type |
| > smpGroup | integer | Smp group ID. If the UID has no group, it is 0 by default |
| > smpOrderId | string | The counterparty's orderID which triggers this SMP execution |
| > createdTime | string | Order created timestamp (ms) |
| > updatedTime | string | Order updated timestamp (ms) |

---


Request Example

HTTP
 
  
  
```http
GET /v5/order/realtime?symbol=ETHUSDT&category=linear&openOnly=0&limit=1  HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672219525810
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "orderId": "fd4300ae-7847-404e-b947-b46980a4d140",
                "orderLinkId": "test-000005",
                "blockTradeId": "",
                "symbol": "ETHUSDT",
                "price": "1600.00",
                "qty": "0.10",
                "side": "Buy",
                "isLeverage": "",
                "positionIdx": 1,
                "orderStatus": "New",
                "cancelType": "UNKNOWN",
                "rejectReason": "EC_NoError",
                "avgPrice": "0",
                "leavesQty": "0.10",
                "leavesValue": "160",
                "cumExecQty": "0.00",
                "cumExecValue": "0",
                "cumExecFee": "0",
                "timeinfo
rce": "GTC",
                "orderType": "Limit",
                "stopOrderType": "UNKNOWN",
                "orderIv": "",
                "triggerPrice": "0.00",
                "takeProfit": "2500.00",
                "stopLoss": "1500.00",
                "tpTriggerBy": "LastPrice",
                "slTriggerBy": "LastPrice",
                "triggerDirection": 0,
                "triggerBy": "UNKNOWN",
                "lastPriceOnCreated": "",
                "reduceOnly": false,
                "closeOnTrigger": false,
                "smpType": "None",
                "smpGroup": 0,
                "smpOrderId": "",
                "tpslMode": "Full",
                "tpLimitPrice": "",
                "slLimitPrice": "",
                "placeType": "",
                "createdTime": "1684738540559",
                "updatedTime": "1684738540561"
            }
        ],
        "nextPageCursor": "page_args%3Dfd4300ae-7847-404e-b947-b46980a4d140%26symbol%3D6%26",
        "category": "linear"
    },
    "retExtinfo
": {},
    "time": 1684765770483
}
```

