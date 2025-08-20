# Create Sub UID
Create a new sub user id. Use master account's api key.


tip

The API key must have one of the below permissions in order to call this endpoint

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"

HTTP Request
```http
POST /v5/user/create-sub-member
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| username | true | string | Give a username of the new sub user id. <br> 6-16 characters, must include both numbers and letters. <br> cannot be the same as the exist or deleted one. |
| password | false | string | Set the password for the new sub user id. <br> 8-30 characters, must include numbers, upper and lowercase letters. |
| memberType | true | integer | 1: normal sub account, 6: custodial sub account |
| switch | false | integer | 0: turn off quick login (default) <br> 1: turn on quick login. |
| isUta | false | boolean | deprecated param, always UTA account |
| note | false | string | Set a remark |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| uid | string | Sub user Id |
| username | string | Give a username of the new sub user id. <br> 6-16 characters, must include both numbers and letters. <br> cannot be the same as the exist or deleted one. |
| memberType | integer | 1: normal sub account, 6: custodial sub account |
| status | integer | The status of the user account <br> 1: normal <br> 2: login banned <br> 4: frozen |
| remark | string | The remark |

---

Request Example

HTTP
 
  
```http
POST /v5/user/create-sub-member HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676429344202
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "username": "xxxxx",
    "memberType": 1,
    "switch": 1,
    "note
": "test"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "uid": "53888000",
        "username": "xxxxx",
        "memberType": 1,
        "status": 1,
        "remark": "test"
    },
    "retExtinfo
": {},
    "time": 1676429344734
}
```

