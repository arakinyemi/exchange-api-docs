# Get Max. Allowed Collateral Reduction Amount
Query for the maximum amount by which collateral may be reduced by.

Permission: "Spot trade"


HTTP Request
```http
GET /v5/crypto-loan/max-collateral-amount
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| orderId | true | string | Loan coin ID |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| maxCollateralAmount | string | Max. reduction collateral amount |

---

Request Example

HTTP
 
  
```http
GET /v5/crypto-loan/max-collateral-amount?orderId=1794267532472646144 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728634289933
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "maxCollateralAmount": "0.00210611"
    },
    "retExtinfo
": {},
    "time": 1728634291554
}
```

