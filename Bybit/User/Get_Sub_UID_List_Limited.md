# Get Sub UID List (Limited)
Get at most 10k sub UID of master account. Use master user's api key only.


tip

The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"

HTTP Request
```http
GET /v5/user/query-sub-members
```

Request Parameters
None



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| subMembers | array | Object |
| > uid | string | Sub user Id |
| > username | string | Username |
| > memberType | integer | 1: normal sub account, 6: custodial sub account |
| > status | integer | The status of the user account <br> 1: normal <br> 2: login banned <br> 4: frozen |
| > accountMode | integer | The account mode of the user account |
| 1: classic account |
| 3: UTA |
| > remark | string | The remark |

---

Request Example

HTTP
 
  
```http
GET /v5/user/query-sub-members HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676430318405
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "subMembers": [
            {
                "uid": "53888001",
                "username": "xxx001",
                "memberType": 1,
                "status": 1,
                "remark": "test",
                "accountMode": 3
            },
            {
                "uid": "53888002",
                "username": "xxx002",
                "memberType": 6,
                "status": 1,
                "remark": "",
                "accountMode": 1
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1676430319452
}
```

