# Amend Order
info

You can only modify unfilled or partially filled orders.


HTTP Request
```http
POST /v5/order/amend
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| symbol | true | string | Spread combination symbol name |
| orderId | false | string | Spread combination order ID. Either orderId or orderLinkId is required |
| orderLinkId | false | string | User customised order ID. Either orderId or orderLinkId is required |
| orderIv      | false | string | Implied volatility. option only. Pass the real value, e.g for 10%, 0.1 should be passed|
| triggerPrice | false | string | For Perps & Futures, it is the conditional order trigger price. If you expect the price to rise to trigger your conditional order, make sure:<br>triggerPrice > market price<br>Else, triggerPrice < market price<br>For spot, it is the TP/SL and Conditional order trigger price |
| qty          | false | string | Order quantity after modification. Do not pass it if not modify the qty                                                                                                                                                                                                            |
| price        | false | string | Order price after modification. Do not pass it if not modify the price                                                                                                                                                                                                             |
| tpslMode     | false | string | TP/SL mode<br>Full: entire position for TP/SL. Then, tpOrderType or slOrderType must be Market<br>Partial: partial position tp/sl. Limit TP/SL order are supported. Note
: When create limit tp/sl, tpslMode is required and it must be PartialValid for linear & inverse           |
| takeProfit   | false | string | Take profit price after modification. If pass "0", it means cancel the existing take profit of the order. Do not pass it if you do not want to modify the take profit. valid for spot(UTA), linear, inverse                                                                        |
| stopLoss     | false | string | Stop loss price after modification. If pass "0", it means cancel the existing stop loss of the order. Do not pass it if you do not want to modify the stop loss. valid for spot(UTA), linear, inverse                                                                              |
| tpTriggerBy  | false | string | The price type to trigger take profit. When set a take profit, this param is required if no initial value for the order                                                                                                                                                            |
| slTriggerBy  | false | string | The price type to trigger stop loss. When set a take profit, this param is required if no initial value for the order                                                                                                                                                              |
| triggerBy    | false | string | Trigger price type                                                                                                                                                                                                                                                                 |
| tpLimitPrice | false | string | Limit order price when take profit is triggered. Only working when original order sets partial limit tp/sl. valid for spot(UTA), linear, inverse                                                                                                                                   |
| slLimitPrice | false | string | Limit order price when stop loss is triggered. Only working when original order sets partial limit tp/sl. valid for spot(UTA), linear, inverse|

info
 

The acknowledgement of an amend order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status. 

---



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| orderId | string | Order ID |
| orderLinkId | string | User customised order ID |

---

Request Example
```http
POST /v5/order/amend HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672217108106
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "category": "linear",
    "symbol": "ETHPERP",
    "orderLinkId": "linear-004",
    "triggerPrice": "1145",
    "qty": "0.15",
    "price": "1050",
    "takeProfit": "0",
    "stopLoss": "0"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderId": "c6f055d9-7f21-4079-913d-e6523a9cfffa",
        "orderLinkId": "linear-004"
    },
    "retExtinfo
": {},
    "time": 1672217093461
}
```

