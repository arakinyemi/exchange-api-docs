# Get Status And Leverage
Query the Spot margin status and leverage of Unified account

Covers: Margin trade (Unified Account)


HTTP Request
```http
GET /v5/spot-margin-trade/state
```

Request Parameters
None



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| spotLeverage | string | Spot margin leverage. Returns "" if the margin trade is turned off |
| spotMarginMode | string | Spot margin status. 1: on, 0: off |
| effectiveLeverage | string | actual leverage ratio. Precision retains 2 decimal places, truncate downwards |

---


Request Example

HTTP
 
  
```http
GET /v5/spot-margin-trade/state HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1692696840996
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "spotLeverage": "10",
        "spotMarginMode": "1",
        "effectiveLeverage": "1"
    },
    "retExtinfo
": {},
    "time": 1692696841231
}
```

