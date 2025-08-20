# Get MMP State

HTTP Request
```http
GET /v5/account/mmp-state
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| baseCoin | true | string | Base coin, uppercase only |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| result | array | Object |
| > baseCoin | string | Base coin |
| > mmpEnabled | boolean | Whether the account is enabled mmp |
| > window | string | Time window (ms) |
| > frozenPeriod | string | Frozen period (ms) |
| > qtyLimit | string | Trade qty limit |
| > deltaLimit | string | Delta limit |
| > mmpFrozenUntil | string | Unfreeze timestamp (ms) |
| > mmpFrozen | boolean | Whether the mmp is triggered. <br>true: mmpFrozenUntil is meaningful <br> false: please ignore the value of mmpFrozenUntil |

---

Request Example

HTTP
 
  
```http
POST /v5/account/mmp-reset HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675842997277
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "baseCoin": "ETH"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "result": [
            {
                "baseCoin": "BTC",
                "mmpEnabled": true,
                "window": "5000",
                "frozenPeriod": "100000",
                "qtyLimit": "0.01",
                "deltaLimit": "0.01",
                "mmpFrozenUntil": "1675760625519",
                "mmpFrozen": false
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1675843188984
}
```

