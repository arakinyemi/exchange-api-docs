# Get Borrow Quota (Spot)
Query the available balance for Spot trading and Margin trading


HTTP Request
```http
GET /v5/order/spot-borrow-check
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> UTA2.0, UTA1.0: spot |
| symbol | true | string | Symbol name |
| side | true | string | Transaction side. Buy,Sell |

---


Response Parameters
| Parameter          | Type   |                                                                                                                                                                                                                       Comments                                                                                                                                                                                                                      |
|--------------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| symbol             | string | Symbol name, like BTCUSDT, uppercase only                                                                                                                                                                                                                                                                                                                                                                                                           |
| side               | string | Side                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| maxTradeQty        | string | The maximum base coin qty can be traded<br>If spot margin trade on and symbol is margin trading pair, it returns available balance + max.borrowable quantity = min(The maximum quantity that a single user can borrow on the platform, The maximum quantity that can be borrowed calculated by IMR MMR of UTA account, The available quantity of the platform's capital pool)<br>Otherwise, it returns actual available balance<br>up to 4 decimals |
| maxTradeAmount     | string | The maximum quote coin amount can be traded<br>If spot margin trade on and symbol is margin trading pair, it returns available balance + max.borrowable amount = min(The maximum amount that a single user can borrow on the platform, The maximum amount that can be borrowed calculated by IMR MMR of UTA account, The available amount of the platform's capital pool)<br>Otherwise, it returns actual available balance<br>up to 8 decimals     |
| spotMaxTradeQty    | string | No matter your Spot margin switch on or not, it always returns actual qty of base coin you can trade or you have (borrowable qty is not included), up to 4 decimals                                                                                                                                                                                                                                                                                 |
| spotMaxTradeAmount | string | No matter your Spot margin switch on or not, it always returns actual amount of quote coin you can trade or you have (borrowable amount is not included), up to 8 decimals                                                                                                                                                                                                                                                                          |
| borrowCoin         | string | Borrow coin                                                                                                                                                                                                                                                                                                                                                                                                                                         |
---


Request Example

HTTP
 
  
  
```http
GET /v5/order/spot-borrow-check?category=spot&symbol=BTCUSDT&side=Buy HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672228522214
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "symbol": "BTCUSDT",
        "maxTradeQty": "6.6065",
        "side": "Buy",
        "spotMaxTradeAmount": "9004.75628594",
        "maxTradeAmount": "218014.01330797",
        "borrowCoin": "USDT",
        "spotMaxTradeQty": "0.2728"
    },
    "retExtinfo
": {},
    "time": 1698895841534
}
```

