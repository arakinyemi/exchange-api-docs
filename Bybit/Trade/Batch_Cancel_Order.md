# Batch Cancel Order
This endpoint allows you to cancel more than one open order in a single request.

important
You must specify orderId or orderLinkId.
If orderId and orderLinkId is not matched, the system will process orderId first.
You can cancel unfilled or partially filled orders.
A maximum of 20 orders (option), 20 orders (inverse), 20 orders (linear), 10 orders (spot) can be cancelled per request.

HTTP Request
```http
POST /v5/order/cancel-batch
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type |
| UTA2.0: linear, option, spot, inverse |
| UTA1.0: linear, option, spot |

---

request	true	array	Object
> symbol	true	string	Symbol name, like BTCUSDT, uppercase only
> orderId	false	string	Order ID. Either orderId or orderLinkId is required
> orderLinkId	false	string	User customised order ID. Either orderId or orderLinkId is required

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


The acknowledgement of an cancel order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status. 

---



Request Example

HTTP
 
  

  
```http
POST /v5/order/cancel-batch HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672223356634
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "category": "spot",
    "request": [
        {
            "symbol": "BTCUSDT",
            "orderId": "1666800494330512128"
        },
        {
            "symbol": "ATOMUSDT",
            "orderLinkId": "1666800494330512129"
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
                "category": "spot",
                "symbol": "BTCUSDT",
                "orderId": "1666800494330512128",
                "orderLinkId": "spot-btc-03"
            },
            {
                "category": "spot",
                "symbol": "ATOMUSDT",
                "orderId": "",
                "orderLinkId": "1666800494330512129"
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
                "code": 170213,
                "msg": "Order does not exist."
            }
        ]
    },
    "time": 1713434299047
}
```

