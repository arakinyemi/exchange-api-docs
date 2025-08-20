# Get Insurance Pool
Query for Bybit insurance pool data (BTC/USDT/USDC etc)

info

The isolated insurance pool balance is updated every 1 minute, and shared insurance pool balance is updated every 24 hours

Please note
 that you may receive data from the previous minute. This is due to mul
tip
le backend containers starting at different times, which may cause a slight delay. You can always rely on the latest minute data for accuracy.

HTTP Request
```http
GET /v5/market/insurance
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| coin | false | string | coin, uppercase only. Default: return all insurance coins |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| updatedTime | string | Data updated time (ms) |
| list | array | Object |
| > coin | string | Coin |
| > symbols | string | symbols with "BTCUSDT,ETHUSDT,SOLUSDT" mean these contracts are shared with one insurance pool<br> For an isolated insurance pool, it returns one contract |
| > balance | string | Balance |
| > value | string | USD value |

---


Request Example

HTTP
 
  
  
  
```http
GET /v5/market/insurance?coin=USDT HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "updatedTime": "1714003200000",
        "list": [
            {
                "coin": "USDT",
                "symbols": "MERLUSDT,10000000AIDOGEUSDT,ZEUSUSDT",
                "balance": "902178.57602476",
                "value": "901898.0963091522"
            },
            {
                "coin": "USDT",
                "symbols": "SOLUSDT,OMNIUSDT,AL  USDT",
                "balance": "14454.51626125",
                "value": "14449.515598975464"
            },
            {
                "coin": "USDT",
                "symbols": "XLMUSDT,WUSDT",
                "balance": "23.45018235",
                "value": "22.992864174376344"
            },
            {
                "coin": "USDT",
                "symbols": "AGIUSDT,WIFUSDT",
                "balance": "10002",
                "value": "9998.896846613574"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1714028451228
}
```


