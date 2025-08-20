# Get Fee Rate
Get the trading fee rate.


HTTP Request
```http
GET /v5/account/fee-rate
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type. spot, linear, inverse, option |
| symbol | false | string | Symbol name, like BTCUSDT, uppercase only. Valid for linear, inverse, spot |
| baseCoin | false | string | Base coin, uppercase only. SOL, BTC, ETH. Valid for option |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type. spot, option. Derivatives does not have this field |
| list | array | Object |
| > symbol | string | Symbol name. Keeps "" for Options |
| > baseCoin | string | Base coin. SOL, BTC, ETH <br> Derivatives does not have this field <br> Keeps "" for Spot |
| > takerFeeRate | string | Taker fee rate |
| > makerFeeRate | string | Maker fee rate |

---


Request Example

HTTP
 
  
```http
GET /v5/account/fee-rate?symbol=ETHUSDT HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676360412362
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "symbol": "ETHUSDT",
                "takerFeeRate": "0.0006",
                "makerFeeRate": "0.0001"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1676360412576
}
```

