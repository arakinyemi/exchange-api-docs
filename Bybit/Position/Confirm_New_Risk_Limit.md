# Confirm New Risk Limit
It is only applicable when the user is marked as only reducing positions (please see the isReduceOnly field in the # Get Position info
 interface). After the user actively adjusts the risk level, this interface is called to try to calculate the adjusted risk level, and if it passes (retCode=0), the system will remove the position reduceOnly mark. You are recommended to call # Get Position info
 to check isReduceOnly field.


HTTP Request
```http
POST /v5/position/confirm-pending-mmr
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> Unified account: linear, inverse <br> Classic account: linear, inverse |
| symbol | true | string | Symbol name |

---


Response Parameters
None


Request Example

HTTP
 
  
  
```http
POST /v5/position/confirm-pending-mmr HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1698051123673
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 53
```

```json
{
    "category": "linear",
    "symbol": "BTCUSDT"
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
    "time": 1698051124588
}
```

