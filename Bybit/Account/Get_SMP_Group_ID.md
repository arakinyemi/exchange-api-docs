# Get SMP Group ID
Query the SMP group ID of self match prevention


HTTP Request
```http
GET /v5/account/smp-group
```

Request Parameters
None



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| smpGroup | integer | Smp group ID. If the UID has no group, it is 0 by default |

---

Request Example

HTTP
 
  
```http
GET /v5/account/smp-group HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1702363848192
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "smpGroup": 0
    },
    "retExtinfo
": {},
    "time": 1702363848539
}
```

