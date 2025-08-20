# Set Deposit Account
Set auto transfer account after deposit. The same function as the setting for Deposit on web GUI

info

Your funds will be deposited into FUND wallet by default. You can set the wallet for auto-transfer after deposit by this API.
Only main UID can access.

tip

UTA2.0 has FUND, UNIFIED

UTA1.0 has FUND, UNIFIED, CONTRACT(for inverse derivatives)

Classic account has FUND, CONTRACT(for inverse derivatives and derivatives), SPOT

HTTP Request
```http
POST /v5/asset/deposit/deposit-to-account
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| accountType | true | string | Account type <br> UNIFIED <br> SPOT <br> CONTRACT <br> FUND |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| status | integer | Request result: <br> 1: SUCCESS <br> 0: FAIL |

---


Request Example

HTTP
 
  
```http
POST /v5/asset/deposit/deposit-to-account HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676887913670
X-BAPI-RECV-WINDOW: 50000
Content-Type: application/json
```

```json
{
    "accountType": "CONTRACT"
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
    "time": 1676887914363
}
```

