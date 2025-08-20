# Cancel All Orders
Cancel all open orders


HTTP Request
```http
POST /v5/spread/order/cancel-all
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| symbol | false | string | Spread combination symbol name <br> When a symbol is specified, all orders for that symbol will be cancelled regardless of the cancelAll field.<br> When a symbol is not specified and cancelAll=true, all orders, regardless of the symbol, will be cancelled |
| cancelAll | false | boolean | true, false |

---

info

The acknowledgement of cancel all orders request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array`<object>` |
| > orderId | string | Order ID |
| > orderLinkId | string | User customised order ID |
| success | string | The field can be ignored |

---

Request Example
```http
POST /v5/spread/order/cancel-all HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1744090967121
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 49
```

```json
{
    "symbol": null,
    "cancelAll": true
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
                "orderId": "11ec47f3-f0a2-4b2a-b302-236f2a2d53a2",
                "orderLinkId": ""
            }
        ],
        "success": "1"
    },
    "retExtinfo
": {},
    "time": 1744090940933
}
```

