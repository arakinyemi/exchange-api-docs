# Get Sub UID List (Unlimited)
This API is applicable to the client who has over 10k sub accounts. Use master user's api key only.


tip

The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"

HTTP Request
```http
GET /v5/user/submembers
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| pageSize | false | string | Data size per page. Return up to 100 records per request |
| nextCursor | false | string | Cursor. Use the nextCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| subMembers | array | Object |
| > uid | string | Sub user Id |
| > username | string | Username |
| > memberType | integer | 1: standard sub account, 6: custodial sub account |
| > status | integer | The status of the user account <br> 1: normal <br> 2: login banned <br> 4: frozen |
| > accountMode | integer | The account mode of the user account <br> 1: Classic Account<br> 3: Unified Trading Account |
| > remark | string | The remark |
| nextCursor | string | The next page cursor value. "0" means no more pages |

---

Request Example

HTTP
 
```http
GET /v5/user/submembers?pageSize=1 HTTP/1.1
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
                "uid": "100475023",
                "username": "BybitmcYERjAPmMU",
                "memberType": 1,
                "status": 1,
                "remark": "",
                "accountMode": 1
            }
        ],
        "nextCursor": "126671"
    },
    "retExtinfo
": {},
    "time": 1711695552772
}
```

