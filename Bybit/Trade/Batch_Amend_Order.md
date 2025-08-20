# Batch Amend Order

tip


This endpoint allows you to amend more than one open order in a single request.

You can modify unfilled or partially filled orders. Conditional orders are not supported.
A maximum of 20 orders (option), 20 orders (inverse), 20 orders (linear), 10 orders (spot) can be amended per request.

HTTP Request
```http
POST /v5/order/amend-batch
```

Request Parameters
| Parameter      | Required | Type   | Comments                                                                                                                                                                                                                                                                      |
|----------------|----------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| category       | true     | string | Product type<br>UTA2.0: linear, option, spot, inverse<br>UTA1.0: linear, option, spot                                                                                                                                                                                         |
| request        | true     | array  | Object                                                                                                                                                                                                                                                                        |
| > symbol       | true     | string | Symbol name, like BTCUSDT, uppercase only                                                                                                                                                                                                                                     |
| > orderId      | false    | string | Order ID. Either orderId or orderLinkId is required                                                                                                                                                                                                                           |
| > orderLinkId  | false    | string | User customised order ID. Either orderId or orderLinkId is required                                                                                                                                                                                                           |
| > orderIv      | false    | string | Implied volatility. option only. Pass the real value, e.g for 10%, 0.1 should be passed                                                                                                                                                                                       |
| > triggerPrice | false    | string | For Perps & Futures, it is the conditional order trigger price. If you expect the price to rise to trigger your conditional order, make sure:<br>triggerPrice > market price<br>Else, triggerPrice < market price<br>For spot, it is for tpslOrder or stopOrder trigger price |
| > qty          | false    | string | Order quantity after modification. Do not pass it if not modify the qty                                                                                                                                                                                                       |
| > price        | false    | string | Order price after modification. Do not pass it if not modify the price                                                                                                                                                                                                        |
| > tpslMode     | false    | string | TP/SL mode<br>Full: entire position for TP/SL. Then, tpOrderType or slOrderType must be Market<br>Partial: partial position tp/sl. Limit TP/SL order are supported. Note
: When create limit tp/sl, tpslMode is required and it must be Partial                                |
| > takeProfit   | false    | string | Take profit price after modification. If pass "0", it means cancel the existing take profit of the order. Do not pass it if you do not want to modify the take profit                                                                                                         |
| > stopLoss     | false    | string | Stop loss price after modification. If pass "0", it means cancel the existing stop loss of the order. Do not pass it if you do not want to modify the stop loss                                                                                                               |
| > tpTriggerBy  | false    | string | The price type to trigger take profit. When set a take profit, this param is required if no initial value for the order                                                                                                                                                       |
| > slTriggerBy  | false    | string | The price type to trigger stop loss. When set a take profit, this param is required if no initial value for the order                                                                                                                                                         |
| > triggerBy    | false    | string | Trigger price type                                                                                                                                                                                                                                                            |
| > tpLimitPrice | false    | string | Limit order price when take profit is triggered. Only working when original order sets partial limit tp/sl                                                                                                                                                                    |
| > slLimitPrice | false    | string | Limit order price when stop loss is triggered. Only working when original order sets partial limit tp/sl                                                                                                                                                                      |

Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| result | Object |
| > list | array | Object |
| >> category | string | Product type |
| >> symbol | string | Symbol name |
| >> orderId | string | Order ID |
| >> orderLinkId | string | User customised order ID |
| retExtinfo
 | Object |
| > list | array | Object |
| >> code | number | Success/error code |
| >> msg | string | Success/error message |

info


The acknowledgement of an amend order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status. 

---



Request Example

HTTP
 
  

  
```http
POST /v5/order/amend-batch HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672222935987
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "category": "option",
    "request": [
        {
            "symbol": "ETH-30DEC22-500-C",
            "qty": null,
            "price": null,
            "orderIv": "6.8",
            "orderId": "b551f227-7059-4fb5-a6a6-699c04dbd2f2"
        },
        {
            "symbol": "ETH-30DEC22-700-C",
            "qty": null,
            "price": "650",
            "orderIv": null,
            "orderId": "fa6a595f-1a57-483f-b9d3-30e9c8235a52"
        }
    ]
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "category": "option",
                "symbol": "ETH-30DEC22-500-C",
                "orderId": "b551f227-7059-4fb5-a6a6-699c04dbd2f2",
                "orderLinkId": ""
            },
            {
                "category": "option",
                "symbol": "ETH-30DEC22-700-C",
                "orderId": "fa6a595f-1a57-483f-b9d3-30e9c8235a52",
                "orderLinkId": ""
            }
        ]
    },
    "retExtinfo
": {
        "list": [
            {
                "code": 0,
                "msg": "OK"
            },
            {
                "code": 0,
                "msg": "OK"
            }
        ]
    },
    "time": 1672222808060
}
```

