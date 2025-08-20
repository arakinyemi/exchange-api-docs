# Get Order History
info

orderId & orderLinkId has a higher priority than startTime & endTime
Fully cancelled orders are stored for up to 24 hours.
Single leg orders can also be found with "createType"=CreateByFutureSpread via # Get Order History


HTTP Request
```http
GET /v5/order/history
```

Request Parameters
| Parameter   | Required | Type    |                                                                                                                                                         Comments                                                                                                                                                        |
|-------------|----------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| category    | true     | string  | Product type<br>UTA2.0, UTA1.0: linear, inverse, spot, option<br>classic account: linear, inverse, spot                                                                                                                                                                                                                 |
| symbol      | false    | string  | Symbol name, like BTCUSDT, uppercase only                                                                                                                                                                                                                                                                               |
| baseCoin    | false    | string  | Base coin, uppercase only<br>UTA1.0(inverse), classic account do not support this param                                                                                                                                                                                                                                 |
| settleCoin  | false    | string  | Settle coin, uppercase only<br>UTA1.0(inverse), classic account do not support this param                                                                                                                                                                                                                               |
| orderId     | false    | string  | Order ID                                                                                                                                                                                                                                                                                                                |
| orderLinkId | false    | string  | User customised order ID                                                                                                                                                                                                                                                                                                |
| orderFilter | false    | string  | Order: active order<br>StopOrder: conditional order for Futures and Spot<br>tpslOrder: spot TP/SL order<br>OcoOrder: spot OCO orders<br>BidirectionalTpslOrder: Spot bidirectional TPSL order<br>classic account spot: return Order active order by default<br>Others: all kinds of orders by default                   |
| orderStatus | false    | string  | Classic spot: not supported<br>UTA2.0, UTA1.0(except inverse): return all closed status orders if not passed<br>UTA1.0(inverse), classic account(linear, inverse): return all status orders if not passed                                                                                                               |
| startTime   | false    | integer | The start timestamp (ms)<br>startTime and endTime are not passed, return 7 days by default<br>Only startTime is passed, return range between startTime and startTime+7 days<br>Only endTime is passed, return range between endTime-7 days and endTime<br>If both are passed, the rule is endTime - startTime <= 7 days |
| endTime     | false    | integer | The end timestamp (ms)                                                                                                                                                                                                                                                                                                  |
| limit       | false    | integer | Limit for data size per page. [1, 50]. Default: 20                                                                                                                                                                                                                                                                      |
| cursor      | false    | string  | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set                                                                                                                                                                                                                      |
---


Response Parameters
| Parameter               | Type    |                                                                                                                   Comments                                                                                                                   |   |
|-------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-|
| category                | string  | Product type                                                                                                                                                                                                                                 |   |
| list                    | array   | Object                                                                                                                                                                                                                                       |   |
| > orderId               | string  | Order ID                                                                                                                                                                                                                                     |   |
| > orderLinkId           | string  | User customised order ID                                                                                                                                                                                                                     |   |
| > blockTradeId          | string  | Block trade ID                                                                                                                                                                                                                               |   |
| > symbol                | string  | Symbol name                                                                                                                                                                                                                                  |   |
| > price                 | string  | Order price                                                                                                                                                                                                                                  |   |
| > qty                   | string  | Order qty                                                                                                                                                                                                                                    |   |
| > side                  | string  | Side. Buy,Sell                                                                                                                                                                                                                               |   |
| > isLeverage            | string  | Whether to borrow. Unified spot only. 0: false, 1: true. . Classic spot is not supported, always 0                                                                                                                                           |   |
| > positionIdx           | integer | Position index. Used to identify positions in different position modes                                                                                                                                                                       |   |
| > orderStatus           | string  | Order status                                                                                                                                                                                                                                 |   |
| > createType            | string  | Order create type<br>Only for category=linear or inverse<br>Spot, Option do not have this key                                                                                                                                                |   |
| > cancelType            | string  | Cancel type                                                                                                                                                                                                                                  |   |
| > rejectReason          | string  | Reject reason. Classic spot is not supported                                                                                                                                                                                                 |   |
| > avgPrice              | string  | Average filled price<br>UTA: returns "" for those orders without avg price<br>classic account: returns "0" for those orders without avg price, and also for those orders have partilly filled but cancelled at the end                       |   |
| > leavesQty             | string  | The remaining qty not executed. Classic spot is not supported                                                                                                                                                                                |   |
| > leavesValue           | string  | The estimated value not executed. Classic spot is not supported                                                                                                                                                                              |   |
| > cumExecQty            | string  | Cumulative executed order qty                                                                                                                                                                                                                |   |
| > cumExecValue          | string  | Cumulative executed order value. Classic spot is not supported                                                                                                                                                                               |   |
| > cumExecFee            | string  | Cumulative executed trading fee. Classic spot is not supported                                                                                                                                                                               |   |
| > timeinfo
rce           | string  | Time in force                                                                                                                                                                                                                                |   |
| > orderType             | string  | Order type. Market,Limit. For TP/SL order, it means the order type after triggered<br>Block trade Roll Back, Block trade-Limit: Unique enum values for Unified account block trades                                                          |   |
| > stopOrderType         | string  | Stop order type                                                                                                                                                                                                                              |   |
| > orderIv               | string  | Implied volatility                                                                                                                                                                                                                           |   |
| > marketUnit            | string  | The unit for qty when create Spot market orders for UTA account. baseCoin, quoteCoin                                                                                                                                                         |   |
| > slippageToleranceType | string  | Spot and Futures market order slippage tolerance type TickSize, Percent, UNKNOWN(default)                                                                                                                                                    |   |
| > slippageTolerance     | string  | Slippage tolerance value                                                                                                                                                                                                                     |   |
| > triggerPrice          | string  | Trigger price. If stopOrderType=TrailingStop, it is activate price. Otherwise, it is trigger price                                                                                                                                           |   |
| > takeProfit            | string  | Take profit price                                                                                                                                                                                                                            |   |
| > stopLoss              | string  | Stop loss price                                                                                                                                                                                                                              |   |
| > tpslMode              | string  | TP/SL mode, Full: entire position for TP/SL. Partial: partial position tp/sl. Spot does not have this field, and Option returns always ""                                                                                                    |   |
| > ocoTriggerBy          | string  | The trigger type of Spot OCO order.OcoTriggerByUnknown, OcoTriggerByTp, OcoTriggerBySl. Classic spot is not supported                                                                                                                        |   |
| > tpLimitPrice          | string  | The limit order price when take profit price is triggered                                                                                                                                                                                    |   |
| > slLimitPrice          | string  | The limit order price when stop loss price is triggered                                                                                                                                                                                      |   |
| > tpTriggerBy           | string  | The price type to trigger take profit                                                                                                                                                                                                        |   |
| > slTriggerBy           | string  | The price type to trigger stop loss                                                                                                                                                                                                          |   |
| > triggerDirection      | integer | Trigger direction. 1: rise, 2: fall                                                                                                                                                                                                          |   |
| > triggerBy             | string  | The price type of trigger price                                                                                                                                                                                                              |   |
| > lastPriceOnCreated    | string  | Last price when place the order, Spot is not applicable                                                                                                                                                                                      |   |
| > basePrice             | string  | Last price when place the order, Spot has this field only                                                                                                                                                                                    |   |
| > reduceOnly            | boolean | Reduce only. true means reduce position size                                                                                                                                                                                                 |   |
| > closeOnTrigger        | boolean | Close on trigger. What is a close on trigger order?                                                                                                                                                                                          |   |
| > placeType             | string  | Place type, option used. iv, price                                                                                                                                                                                                           |   |
| > smpType               | string  | SMP execution type                                                                                                                                                                                                                           |   |
| > smpGroup              | integer | Smp group ID. If the UID has no group, it is 0 by default                                                                                                                                                                                    |   |
| > smpOrderId            | string  | The counterparty's orderID which triggers this SMP execution                                                                                                                                                                                 |   |
| > createdTime           | string  | Order created timestamp (ms)                                                                                                                                                                                                                 |   |
| > updatedTime           | string  | Order updated timestamp (ms)                                                                                                                                                                                                                 |   |
| > extraFees             | string  | Trading fee rate information. Currently, this data is returned only for spot orders placed on the Indonesian site or spot fiat currency orders placed on the EU site. In other cases, an empty string is returned. Enum: feeType, subFeeType |   |
| nextPageCursor          | string  | Refer to the cursor request parameter                                                                                                                                                                                                        |   |

---

Request Example
```http
GET /v5/order/history?category=linear&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672221263407
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
                "orderId": "14bad3a1-6454-43d8-bcf2-5345896cf74d",
                "orderLinkId": "YLxaWKMiHU",
                "blockTradeId": "",
                "symbol": "BTCUSDT",
                "price": "26864.40",
                "qty": "0.003",
                "side": "Buy",
                "isLeverage": "",
                "positionIdx": 1,
                "orderStatus": "Cancelled",
                "cancelType": "UNKNOWN",
                "rejectReason": "EC_PostOnlyWillTakeLiquidity",
                "avgPrice": "0",
                "leavesQty": "0.000",
                "leavesValue": "0",
                "cumExecQty": "0.000",
                "cumExecValue": "0",
                "cumExecFee": "0",
                "timeinfo
rce": "PostOnly",
                "orderType": "Limit",
                "stopOrderType": "UNKNOWN",
                "orderIv": "",
                "triggerPrice": "0.00",
                "takeProfit": "0.00",
                "stopLoss": "0.00",
                "tpTriggerBy": "UNKNOWN",
                "slTriggerBy": "UNKNOWN",
                "triggerDirection": 0,
                "triggerBy": "UNKNOWN",
                "lastPriceOnCreated": "0.00",
                "reduceOnly": false,
                "closeOnTrigger": false,
                "smpType": "None",
                "smpGroup": 0,
                "smpOrderId": "",
                "tpslMode": "",
                "tpLimitPrice": "",
                "slLimitPrice": "",
                "placeType": "",
                "slippageToleranceType": "UNKNOWN",
                "slippageTolerance": "",
                "createdTime": "1684476068369",
                "updatedTime": "1684476068372",
                "extraFees": ""
            }
        ],
        "nextPageCursor": "page_token%3D39380%26",
        "category": "linear"
    },
    "retExtinfo
": {},
    "time": 1684766282976
}
```

