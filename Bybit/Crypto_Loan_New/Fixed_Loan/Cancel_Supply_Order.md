# Cancel Supply Order
Permission: "Spot trade"

UID rate limit: 1 req / second


HTTP Request
```http
POST /v5/crypto-loan-fixed/supply-order-cancel
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| orderId | true | string | Order ID of fixed supply order |

---


Response Parameters
None


Request Example

HTTP
 
  
```http
POST /v5/crypto-loan-fixed/supply-order-cancel HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752652612736
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 26
```

```json
{
    "orderId": "13577"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {},
    "retExtinfo
": {},
    "time": 1752652613638
}
```

