# Delete Sub UID
Delete a sub UID. Before deleting the UID, please make sure there is no asset.
Use master user's api key**.


tip

The API key must have one of the below permissions in order to call this endpoint

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"

HTTP Request
```http
POST /v5/user/del-submember
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| subMemberId | true | string | Sub UID |

---


Response Parameters
None


Request Example

HTTP
 
  
```http
POST /v5/user/del-submember HTTP/1.1
Host: api.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1698907012755
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
Content-Type: application/json
Content-Length: 34
```

```json
{
    "subMemberId": "112725187"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtinfo
": {},
    "time": 1698907012962
}
```

