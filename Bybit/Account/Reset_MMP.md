# Reset MMP
info

Once the mmp triggered, you can unfreeze the account by this endpoint, then qtyLimit and deltaLimit will be reset to 0.
If the account is not frozen, reset action can also remove previous accumulation, i.e., qtyLimit and deltaLimit will be reset to 0.

HTTP Request
```http
POST /v5/account/mmp-reset
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| baseCoin | true | string | Base coin, uppercase only |

---


Response Parameters
None


Request Example

HTTP
 
  
```http
POST /v5/account/mmp-reset HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675842997277
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "baseCoin": "ETH"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success"
}
```

