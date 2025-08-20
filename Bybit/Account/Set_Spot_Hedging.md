# Set Spot Hedging
You can turn on/off Spot hedging feature in Portfolio margin for Unified account


HTTP Request
```http
POST /v5/account/set-hedging-mode
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| setHedgingMode | true | string | ON, OFF |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| retCode | integer | Result code |
| retMsg | string | Result message |

---


Request Example

HTTP
 
  
```http
POST /v5/account/set-hedging-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1700117968580
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 31
```

```json
{
    "setHedgingMode": "OFF"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "SUCCESS"
}
```

