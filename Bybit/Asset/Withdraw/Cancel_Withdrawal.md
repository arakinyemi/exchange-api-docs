# Cancel Withdrawal
Cancel the withdrawal


HTTP Request
```http
POST /v5/asset/withdraw/cancel
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| id | true | string | Withdrawal ID |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| status | integer | 0: fail. 1: success |

---

Request Example

HTTP
 
  
```http
POST /v5/asset/withdraw/cancel HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672197227732
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
Content-Type: application/json
```

```json
{
    "id": "10197"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "status": 1
    },
    "retExtinfo
": {},
    "time": 1672197228408
}
```

