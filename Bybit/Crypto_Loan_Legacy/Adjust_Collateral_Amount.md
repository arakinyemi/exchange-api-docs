# Adjust Collateral Amount
You can increase or reduce your collateral amount. When you reduce, please obey the max. allowed reduction amount.

Permission: "Spot trade"

info

The adjusted collateral amount will be returned to or deducted from the Funding wallet.

HTTP Request
```http
POST /v5/crypto-loan/adjust-ltv
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| orderId | true | string | Loan order ID |
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
POST /v5/crypto-loan/adjust-ltv HTTP/1.1
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
    "orderId": "1794267532472646144",
    "amount": "0.001",
    "direction": "1"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "adjustId": "1794318409405331968"
    },
    "retExtinfo
": {},
    "time": 1728635422833
}
```

