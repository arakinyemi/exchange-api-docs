# Bind Or Unbind UID
For the institutional loan product, you can bind new UIDs to the risk unit or unbind UID from the risk unit.

info

The risk unit designated UID cannot be unbound.
The UID you want to bind must be upgraded to UTA Pro.

HTTP Request
```http
POST /v5/ins-loan/association-uid
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| uid | true | string | UID <br> Bind <br> a) the key used must be from one of UIDs in the risk unit; <br> b) input UID must not have an INS loan <br> Unbind <br> a) the key used must be from one of UIDs in the risk unit; <br> b) input UID cannot be the same as the UID used to access the API |
| operate | true | string | 0: bind, 1: unbind |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| uid | string | UID |
| operate | string | 0: bind, 1: unbind |

---

Request Example

HTTP
 
  
```http
POST /v5/ins-loan/association-uid HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1699257853101
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
Content-Type: application/json
Content-Length: 43
```

```json
{
    "uid": "592324",
    "operate": "0"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "uid": "592324",
        "operate": "0"
    },
    "retExtinfo
": {},
    "time": 1699257746135
}
```

