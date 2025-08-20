# Freeze Sub UID
Freeze Sub UID. Use master user's api key only.


tip

The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"

HTTP Request
```http
POST /v5/user/frozen-sub-member
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| subuid | true | integer | Sub user Id |
| frozen | true | integer | 0：unfreeze, 1：freeze |

---


Response Parameters
None


Request Example

HTTP
 
  
```http
POST /v5/user/frozen-sub-member HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676430842094
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "subuid": 53888001,
    "frozen": 1
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {},
    "retExtinfo
": {},
    "time": 1676430697553
}
```

