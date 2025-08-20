# Confirm a Quote
info

The exchange is async; please check the final status by calling the query result API.
Make sure you confirm the quote before it expires.

HTTP Request
```http
POST /v5/asset/exchange/convert-execute
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| quoteTxId | true | string | The quote tx ID from Request a Quote |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| quoteTxId | string | Quote transaction ID |
| exchangeStatus | string | Exchange status <br> init <br> processing <br> success <br> failure |

---

Request Example

HTTP
 
  
```http
POST /v5/asset/exchange/convert-execute HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1720071899789
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 52
```

```json
{
    "quoteTxId": "10100108106409343501030232064"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "exchangeStatus": "processing",
        "quoteTxId": "10100108106409343501030232064"
    },
    "retExtinfo
": {},
    "time": 1720071900529
}
```

