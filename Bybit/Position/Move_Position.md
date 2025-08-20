# Move Position
You can move positions between sub-master, master-sub, or sub-sub UIDs when necessary


tip

UTA2.0 inverse contract move position is not supported for now

info

The endpoint can only be called by master UID api key
UIDs must be the same master-sub account relationship
The trades generated from move-position endpoint will not be displayed in the Recent Trade (Rest API & Websocket)
There is no trading fee
fromUid and toUid both should be Unified trading accounts, and they need to be one-way mode when moving the positions
Please note
 that once executed, you will get execType=MovePosition entry from # Get Trade History, # Get Closed Pnl, and stream from Execution.

HTTP Request
```http
POST /v5/position/move-positions
```

Request Parameters
| Parameter  | Required | Type   | Comments                                                                                                                                                                                         |
|------------|----------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fromUid    | true     | string | From UID<br>Must be UTA<br>Must be in one-way mode for Futures                                                                                                                                   |
| toUid      | true     | string | To UID<br>Must be UTA<br>Must be in one-way mode for Futures                                                                                                                                     |
| list       | true     | array  | Object. Up to 25 legs per request                                                                                                                                                                |
| > category | true     | string | Product type<br>UTA2.0, UTA1.0: linear, spot, option                                                                                                                                             |
| > symbol   | true     | string | Symbol name, like BTCUSDT, uppercase only                                                                                                                                                        |
| > price    | true     | string | Trade price<br>linear: the price needs to be between [95% of mark price, 105% of mark price]<br>spot&option: the price needs to follow the price rule from Instruments Info                      |
| > side     | true     | string | Trading side of fromUid<br>For example, fromUid has a long position, when side=Sell, then once executed, the position of fromUid will be reduced or open a short position depending on qty input |
| > qty      | true     | string | Executed qty<br>The value must satisfy the qty rule from Instruments Info, in particular, category=linear is able to input maxOrderQty * 5                                                       |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| retCode | integer | Result code. 0 means request is successfully accepted |
| retMsg | string | Result message |
| result | map | Object |
| > blockTradeId | string | Block trade ID |
| > status | string | Status. Processing, Rejected |
| > rejectParty | string | "" means initial validation is passed, please check the order status via # Get Move Position History <br> Taker, Maker when status=Rejected <br> bybit means error is occurred on the Bybit side |

---

Request Example

HTTP
 
  
  
```http
POST /v5/position/move-positions HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1697447928051
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "fromUid": "100307601",
    "toUid": "592324",
    "list": [
        {
            "category": "spot",
            "symbol": "BTCUSDT",
            "price": "100",
            "side": "Sell",
            "qty": "0.01"
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
        "blockTradeId": "e9bb926c95f54cf1ba3e315a58b8597b",
        "status": "Processing",
        "rejectParty": ""
    }
}
```

