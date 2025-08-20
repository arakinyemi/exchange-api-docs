# Execution

Subscribe to the execution stream to see your executions in real-time.

> **tip**  
> You may have multiple executions for one order in a single message.

**All-In-One Topic:** execution  
**Categorised Topic:** execution.spot, execution.linear, execution.inverse, execution.option

> **info**  
> All-In-One topic and Categorised topic cannot be in the same subscription request  
> All-In-One topic: Allow you to listen to all categories (spot, linear, inverse, option) websocket updates  
> Categorised Topic: Allow you to listen only to specific category websocket updates

### Response Parameters

| Parameter         | Type   | Comments                                                                        |
|-------------------|--------|---------------------------------------------------------------------------------|
| id                | string | Message ID                                                                      |
| topic             | string | Topic name                                                                      |
| creationTime      | number | Data created timestamp (ms)                                                     |
| data              | array  | Object                                                                          |

Inside `data` array objects:

| Parameter       | Type    | Comments                                                                                     |
|-----------------|---------|----------------------------------------------------------------------------------------------|
| category        | string  | Product type  <br>UTA2.0, UTA1.0: spot, linear, inverse, option  <br>Classic account: spot, linear, inverse                                                          |
| symbol          | string  | Symbol name                                                                                  |
| isLeverage      | string  | Whether to borrow. Unified spot only. 0: false, 1: true  <br>Classic spot not supported, always 0                                                             |
| orderId         | string  | Order ID                                                                                     |
| orderLinkId     | string  | User customized order ID                                                                     |
| side            | string  | Side. Buy, Sell                                                                             |
| orderPrice      | string  | Order price. Classic spot not supported                                                     |
| orderQty        | string  | Order qty. Classic spot not supported                                                       |
| leavesQty       | string  | Remaining qty not executed. Classic spot not supported                                      |
| createType      | string  | Order create type  <br>Classic & UTA1.0(category=inverse): always `""`  <br>Spot, Option do not have this key                                                                |
| orderType       | string  | Order type. Market, Limit. Classic spot not supported                                        |
| stopOrderType   | string  | Stop order type if order is stop order, else not returned. Classic spot not supported       |
| execFee         | string  | Executed trading fee. Classic spot not supported                                            |
| execId          | string  | Execution ID                                                                               |
| execPrice       | string  | Execution price                                                                            |
| execQty         | string  | Execution quantity                                                                         |
| execPnl         | string  | Profit and Loss for each close position execution. Matches the "cashFlow" field in Get Transaction Log |
| execType        | string  | Execution type. Classic spot not supported                                                  |
| execValue       | string  | Executed order value. Classic spot not supported                                           |
| execTime        | string  | Executed timestamp (ms)                                                                   |
| isMaker         | boolean | Is maker order. true: maker, false: taker                                                 |
| feeRate         | string  | Trading fee rate. Classic spot not supported                                              |
| tradeIv         | string  | Implied volatility. Valid for option                                                      |
| markIv          | string  | Implied volatility of mark price. Valid for option                                        |
| markPrice       | string  | Mark price of symbol when executing. Valid for option                                    |
| indexPrice      | string  | Index price of symbol when executing. Valid for option                                   |
| underlyingPrice | string  | Underlying price of symbol when executing. Valid for option                              |
| blockTradeId    | string  | Paradigm block trade ID                                                                  |
| closedSize      | string  | Closed position size                                                                     |
| extraFees       | List    | Extra trading fee information. Only returned for certain KYC contexts; else empty string. Enum: feeType, subFeeType |
| seq             | long    | Cross sequence to associate each fill and position update. Different symbols may share seq, use seq + symbol for unique id  |

### Subscribe Example

```json
{
    "op": "subscribe",
    "args": [
        "execution"
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

ws.execution_stream(callback=handle_message)

while True:
    sleep(1)
```

### Stream Example

```json
{
    "topic": "execution",
    "id": "386825804_BTCUSDT_140612148849382",
    "creationTime": 1746270400355,
    "data": [
        {
            "category": "linear",
            "symbol": "BTCUSDT",
            "closedSize": "0.5",
            "execFee": "26.3725275",
            "execId": "0ab1bdf7-4219-438b-b30a-32ec863018f7",
            "execPrice": "95900.1",
            "execQty": "0.5",
            "execType": "Trade",
            "execValue": "47950.05",
            "feeRate": "0.00055",
            "tradeIv": "",
            "markIv": "",
            "blockTradeId": "",
            "markPrice": "95901.48",
            "indexPrice": "",
            "underlyingPrice": "",
            "leavesQty": "0",
            "orderId": "9aac161b-8ed6-450d-9cab-c5cc67c21784",
            "orderLinkId": "",
            "orderPrice": "94942.5",
            "orderQty": "0.5",
            "orderType": "Market",
            "stopOrderType": "UNKNOWN",
            "side": "Sell",
            "execTime": "1746270400353",
            "isLeverage": "0",
            "isMaker": false,
            "seq": 140612148849382,
            "marketUnit": "",
            "execPnl": "0.05",
            "createType": "CreateByUser",
            "extraFees":[{"feeCoin":"USDT","feeType":"GST","subFeeType":"IND_GST","feeRate":"0.0000675","fee":"0.006403779"}]
        }
    ]
}
```

***

