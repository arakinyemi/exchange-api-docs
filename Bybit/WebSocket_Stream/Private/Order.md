# Order

Subscribe to the order stream to see changes to your orders in real-time.

**All-In-One Topic:** order  
**Categorised Topic:** order.spot, order.linear, order.inverse, order.option

> **info**  
> All-In-One topic and Categorised topic cannot be in the same subscription request  
> All-In-One topic: Allow you to listen to all categories (spot, linear, inverse, option) websocket updates  
> Categorised Topic: Allow you to listen only to specific category websocket updates

> **tip**  
> You may receive two `orderStatus=Filled` messages when the cancel request is accepted but the order is executed at the same time. One message with `rejectReason=EC_NoError` indicates execution, the second with `cancelType=CancelByUser`, `rejectReason=EC_OrigClOrdIDDoesNotExist` indicates cancel request rejection due to order executed.

### Response Parameters

| Parameter       | Type     | Comments                                                                                                           |
|-----------------|----------|--------------------------------------------------------------------------------------------------------------------|
| id              | string   | Message ID                                                                                                         |
| topic           | string   | Topic name                                                                                                         |
| creationTime    | number   | Data created timestamp (ms)                                                                                        |
| data            | array    | Object                                                                                                            |

Inside `data` array objects:

| Parameter          | Type     | Comments                                                                                                      |
|--------------------|----------|---------------------------------------------------------------------------------------------------------------|
| category           | string   | Product type  <br>UTA2.0, UTA1.0: spot, linear, inverse, option  <br>Classic account: spot, linear, inverse                                  |
| orderId            | string   | Order ID                                                                                                    |
| orderLinkId        | string   | User customised order ID                                                                                    |
| isLeverage         | string   | Whether to borrow. Unified spot only. 0: false, 1: true — Classic spot always 0                                 |
| blockTradeId       | string   | Block trade ID                                                                                              |
| symbol             | string   | Symbol name                                                                                                |
| price              | string   | Order price                                                                                                |
| brokerOrderPrice   | string   | Dedicated field for EU liquidity provider                                                                  |
| qty                | string   | Order qty                                                                                                  |
| side               | string   | Side. Buy, Sell                                                                                            |
| positionIdx        | integer  | Position index — identifies positions in different modes                                                     |
| orderStatus        | string   | Order status                                                                                               |
| createType         | string   | Order create type — only for category=linear or inverse. Spot, Option do not have this key                    |
| cancelType         | string   | Cancel type                                                                                                |
| rejectReason       | string   | Reject reason — classic spot not supported                                                                |
| avgPrice           | string   | Average filled price. Returns `""` for orders without avg price or partially filled/cancelled classic orders |
| leavesQty          | string   | Remaining qty not executed — classic spot not supported                                                     |
| leavesValue        | string   | Remaining value not executed — classic spot not supported                                                  |
| cumExecQty         | string   | Cumulative executed order qty                                                                              |
| cumExecValue       | string   | Cumulative executed order value                                                                            |
| cumExecFee         | string   | Cumulative executed trading fee. Classic spot: latest fee for order. After upgrade to Unified, use execFee from Execution topic for fills  |
| closedPnl          | string   | Closed profit and loss for each close position order                                                        |
| feeCurrency        | string   | Trading fee currency for Spot only                                                                          |
| timeInForce        | string   | Time in force                                                                                               |
| orderType          | string   | Order type. Market, Limit. For TP/SL order, means order type after triggered                                 |
| stopOrderType      | string   | Stop order type                                                                                            |
| ocoTriggerBy       | string   | The trigger type of Spot OCO order. Classic spot not supported                                             |
| orderIv            | string   | Implied volatility                                                                                          |
| marketUnit         | string   | Unit for qty for Spot market orders in UTA account (baseCoin, quoteCoin)                                   |
| slippageToleranceType | string | Spot and Futures market order slippage tolerance type: TickSize, Percent, UNKNOWN (default)                  |
| slippageTolerance  | string   | Slippage tolerance value                                                                                     |
| triggerPrice       | string   | Trigger price. For TrailingStop, activate price; else, trigger price                                        |
| takeProfit         | string   | Take profit price                                                                                           |
| stopLoss           | string   | Stop loss price                                                                                            |
| tpslMode           | string   | TP/SL mode — Full: entire position; Partial: partial. Spot does not have this field. Option returns always `""` |
| tpLimitPrice       | string   | Limit order price when take profit triggered                                                              |
| slLimitPrice       | string   | Limit order price when stop loss triggered                                                                |
| tpTriggerBy        | string   | Price type to trigger take profit                                                                          |
| slTriggerBy        | string   | Price type to trigger stop loss                                                                             |
| triggerDirection   | integer  | Trigger direction. 1: rise, 2: fall                                                                        |
| triggerBy          | string   | Price type of trigger price                                                                                 |
| lastPriceOnCreated | string   | Last price when placing the order                                                                           |
| reduceOnly         | boolean  | Reduce only. true means reduce position size                                                               |
| closeOnTrigger     | boolean  | Close on trigger                                                                                                |
| placeType          | string   | Place type, used for options (iv, price)                                                                    |
| smpType            | string   | SMP execution type                                                                                            |
| smpGroup           | integer  | SMP group ID. If none, defaults to 0                                                                         |
| smpOrderId         | string   | Counterparty's orderID which triggered this SMP execution                                                    |
| createdTime        | string   | Order created timestamp (ms)                                                                                  |
| updatedTime        | string   | Order updated timestamp (ms)                                                                                  |

### Subscribe Example

```json
{
    "op": "subscribe",
    "args": [
        "order"
    ]
}
```

```python
from pybit.unified_trading import WebSocket
from time import sleep

ws = WebSocket(
    testnet=True,
    channel_type="private",
    api_key="xxxxxxxxxxxxxxxxxx",
    api_secret="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
)

def handle_message(message):
    print(message)

ws.order_stream(callback=handle_message)

while True:
    sleep(1)
```

### Stream Example

```json
{
    "id": "5923240c6880ab-c59f-420b-9adb-3639adc9dd90",
    "topic": "order",
    "creationTime": 1672364262474,
    "data": [
        {
            "symbol": "ETH-30DEC22-1400-C",
            "orderId": "5cf98598-39a7-459e-97bf-76ca765ee020",
            "side": "Sell",
            "orderType": "Market",
            "cancelType": "UNKNOWN",
            "price": "72.5",
            "qty": "1",
            "orderIv": "",
            "timeInForce": "IOC",
            "orderStatus": "Filled",
            "orderLinkId": "",
            "lastPriceOnCreated": "",
            "reduceOnly": false,
            "leavesQty": "",
            "leavesValue": "",
            "cumExecQty": "1",
            "cumExecValue": "75",
            "avgPrice": "75",
            "blockTradeId": "",
            "positionIdx": 0,
            "cumExecFee": "0.358635",
            "closedPnl": "0",
            "createdTime": "1672364262444",
            "updatedTime": "1672364262457",
            "rejectReason": "EC_NoError",
            "stopOrderType": "",
            "tpslMode": "",
            "triggerPrice": "",
            "takeProfit": "",
            "stopLoss": "",
            "tpTriggerBy": "",
            "slTriggerBy": "",
            "tpLimitPrice": "",
            "slLimitPrice": "",
            "triggerDirection": 0,
            "triggerBy": "",
            "closeOnTrigger": false,
            "category": "option",
            "placeType": "price",
            "smpType": "None",
            "smpGroup": 0,
            "smpOrderId": "",
            "feeCurrency": ""
        }
    ]
}
```


