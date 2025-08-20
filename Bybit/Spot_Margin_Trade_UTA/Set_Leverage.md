# Set Leverage
Set the user's maximum leverage in spot cross margin

Covers: Margin trade (Unified Account)

caution

Your account needs to activate spot margin first; i.e., you must have finished the quiz on web / app.


HTTP Request
```http
POST /v5/spot-margin-trade/set-leverage
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| leverage | true | string | Leverage. [2, 10]. |

---



Response Parameters
None


Request Example

HTTP
 
  
```http
POST /v5/spot-margin-trade/set-leverage HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672299806626
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "leverage": "4"
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
    "time": 1672710944282
}
```

