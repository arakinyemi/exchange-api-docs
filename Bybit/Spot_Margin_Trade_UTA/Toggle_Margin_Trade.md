# Toggle Margin Trade
Turn on / off spot margin trade

Covers: Margin trade (Unified Account)

caution

Your account needs to activate spot margin first; i.e., you must have finished the quiz on web / app.


HTTP Request
```http
POST /v5/spot-margin-trade/switch-mode
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| spotMarginMode | true | string | 1: on, 0: off |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| spotMarginMode | string | Spot margin status. 1: on, 0: off |

---


Request Example

HTTP
 
  
```http
POST /v5/spot-margin-trade/switch-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672297794480
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "spotMarginMode": "0"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "spotMarginMode": "0"
    },
    "retExtinfo
": {},
    "time": 1672297795542
}
```

