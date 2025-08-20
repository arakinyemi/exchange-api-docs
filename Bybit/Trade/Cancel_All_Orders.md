# Cancel All Orders
Cancel all open orders


HTTP Request
```http
POST /v5/order/cancel-all
```

Request Parameters
| Parameter     | Required | Type   | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------|----------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| category      | true     | string | Product type<br>UTA2.0, UTA1.0: linear, inverse, spot, option<br>classic account: linear, inverse, spot                                                                                                                                                                                                                                                                                                                                  |
| symbol        | false    | string | Symbol name, like BTCUSDT, uppercase only<br>linear&inverse: Required if not passing baseCoin or settleCoin                                                                                                                                                                                                                                                                                                                              |
| baseCoin      | false    | string | Base coin, uppercase only<br>linear & inverse(classic account): If cancel all by baseCoin, it will cancel all linear & inverse orders. Required if not passing symbol or settleCoin<br>linear & inverse(Unified account): If cancel all by baseCoin, it will cancel all corresponding category orders. Required if not passing symbol or settleCoin<br>spot(classic account): invalid                                                    |
| settleCoin    | false    | string | Settle coin, uppercase only<br>linear & inverse: Required if not passing symbol or baseCoin<br>option: USDT or USDC<br>Not support spot                                                                                                                                                                                                                                                                                                  |
| orderFilter   | false    | string | category=spot, you can pass Order, tpslOrder, StopOrder, OcoOrder, BidirectionalTpslOrder<br>If not passed, Order by default<br>category=linear or inverse, you can pass Order, StopOrder,OpenOrder<br>If not passed, all kinds of orders will be cancelled, like active order, conditional order, TP/SL order and trailing stop order<br>category=option, you can pass Order<br>No matter it is passed or not, always cancel all orders |
| stopOrderType | false    | string | Stop order type Stop<br>Only used for category=linear or inverse and orderFilter=StopOrder,you can cancel conditional orders except TP/SL order and Trailing stop orders with this param|

info


The acknowledgement of cancel all orders request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status. 

---



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array |Object |
| > orderId | string | Order ID |
| > orderLinkId | string | User customised order ID |
| success | string | "1": success, "0": fail<br> UTA1.0(inverse), classic(linear, inverse) do not return this field |

---

Request Example
```http
POST /v5/order/cancel-all HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1672219779140
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

```

```json
{
  "category": "linear",
  "symbol": null,
  "settleCoin": "USDT"
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
                "orderId": "1616024329462743808",
                "orderLinkId": "1616024329462743809"
            },
            {
                "orderId": "1616024287544869632",
                "orderLinkId": "1616024287544869633"
            }
        ],
        "success": "1"
    },
    "retExtinfo
": {},
    "time": 1707381118116
}
```

