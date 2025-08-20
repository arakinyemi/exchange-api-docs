# Set Trading Stop
Set the take profit, stop loss or trailing stop for the position.


tip


Passing these parameters will create conditional orders by the system internally. The system will cancel these orders if the position is closed, and adjust the qty according to the size of the open position.

info


New version of TP/SL function supports both holding entire position TP/SL orders and holding partial position TP/SL orders.

Full position TP/SL orders: This API can be used to modify the parameters of existing TP/SL orders.
Partial position TP/SL orders: This API can only add partial position TP/SL orders.

note


Under the new version of TP/SL function, when calling this API to perform one-sided take profit or stop loss modification on existing TP/SL orders on the holding position, it will cause the paired tp/sl orders to lose binding relationship. This means that when calling the cancel API through the tp/sl order ID, it will only cancel the corresponding one-sided take profit or stop loss order ID.


HTTP Request
```http
POST /v5/position/trading-stop
```

Request Parameters
| Parameter    | Required | Type    | Comments                                                                                                                         |
|--------------|----------|---------|----------------------------------------------------------------------------------------------------------------------------------|
| category     | true     | string  | Product type<br>UTA2.0, UTA1.0: linear, inverse<br>Classic account: linear, inverse                                              |
| symbol       | true     | string  | Symbol name, like BTCUSDT, uppercase only                                                                                        |
| tpslMode     | true     | string  | TP/SL mode<br>Full: entire position TP/SL<br>Partial: partial position TP/SL                                                     |
| positionIdx  | true     | integer | Used to identify positions in different position modes.<br>0: one-way mode<br>1: hedge-mode Buy side<br>2: hedge-mode Sell side  |
| takeProfit   | false    | string  | Cannot be less than 0, 0 means cancel TP                                                                                         |
| stopLoss     | false    | string  | Cannot be less than 0, 0 means cancel SL                                                                                         |
| trailingStop | false    | string  | Trailing stop by price distance. Cannot be less than 0, 0 means cancel TS                                                        |
| tpTriggerBy  | false    | string  | Take profit trigger price type                                                                                                   |
| slTriggerBy  | false    | string  | Stop loss trigger price type                                                                                                     |
| activePrice  | false    | string  | Trailing stop trigger price. Trailing stop will be triggered when this price is reached only                                     |
| tpSize       | false    | string  | Take profit size<br>valid for TP/SL partial mode, note
: the value of tpSize and slSize must equal                                |
| slSize       | false    | string  | Stop loss size<br>valid for TP/SL partial mode, note
: the value of tpSize and slSize must equal                                  |
| tpLimitPrice | false    | string  | The limit order price when take profit price is triggered. Only works when tpslMode=Partial and tpOrderType=Limit                |
| slLimitPrice | false    | string  | The limit order price when stop loss price is triggered. Only works when tpslMode=Partial and slOrderType=Limit                  |
| tpOrderType  | false    | string  | The order type when take profit is triggered. Market(default), Limit<br>For tpslMode=Full, it only supports tpOrderType="Market" |
| slOrderType  | false    | string  | The order type when stop loss is triggered. Market(default), Limit<br>For tpslMode=Full, it only supports slOrderType="Market"   |

---


Response Parameters
None



Request Example

HTTP
 
  
  
```http
POST /v5/position/trading-stop HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672283124270
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "category":"linear",
    "symbol": "XRPUSDT",
    "takeProfit": "0.6",
    "stopLoss": "0.2",
    "tpTriggerBy": "MarkPrice",
    "slTriggerBy": "IndexPrice",
    "tpslMode": "Partial",
    "tpOrderType": "Limit",
    "slOrderType": "Limit",
    "tpSize": "50",
    "slSize": "50",
    "tpLimitPrice": "0.57",
    "slLimitPrice": "0.21",
    "positionIdx": 0
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtinfo
": {},
    "time": 1672283125359
}
```