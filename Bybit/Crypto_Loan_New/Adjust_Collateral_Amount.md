# Adjust Collateral Amount
You can increase or reduce your collateral amount. When you reduce, please obey the Get Max. Allowed Collateral Reduction Amount

    Permission: "Spot trade"
    UID rate limit: 1 req / second



info

The adjusted collateral amount will be returned to or deducted from the Funding wallet.

HTTP Request
```http
POST /v5/crypto-loan-common/adjust-ltv
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| currency | true | string | Collateral coin |
| amount | true | string | Adjustment amount |
| direction | true | string | 0: add collateral; 1: reduce collateral |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| adjustId | string | Collateral adjustment transaction ID |

---

Request Example

HTTP
 
  
```http
POST /v5/crypto-loan-common/adjust-ltv HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728635421137
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 85
```

```json
{
    "currency": "BTC",
    "amount": "0.008",
    "direction": "1"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "adjustId": 27511
    },
    "retExtInfo": {},
    "time": 1752627997915
}
```

