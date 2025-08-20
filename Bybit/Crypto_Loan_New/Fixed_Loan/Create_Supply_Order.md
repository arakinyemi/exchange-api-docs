# Create Supply Order
Permission: "Spot trade"
UID rate limit: 1 req / second


HTTP Request
```http
POST /v5/crypto-loan-fixed/supply
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| orderCurrency | true | string | Currency to supply |
| orderAmount | true | string | Amount to supply |
| annualRate | true | string | Customizable annual interest rate, e.g., 0.02 means 2% |
| term | true | string | Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| orderId | string | Supply order ID |

---

Request Example

HTTP
 
  
```http
POST /v5/crypto-loan-fixed/supply HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752652261840
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 104
```

```json
{
    "orderCurrency": "USDT",
    "orderAmount": "2002.21",
    "annualRate": "0.35",
    "term": "7"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "orderId": "13007"
    },
    "retExtinfo
": {},
    "time": 1752633650147
}
```

