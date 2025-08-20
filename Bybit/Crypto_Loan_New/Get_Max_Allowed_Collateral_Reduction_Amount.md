# Get Max. Allowed Collateral Reduction Amount
Retrieve the maximum redeemable amount of your collateral asset based on LTV.

    Permission: "Spot trade"

    UID rate limit: 5 req / second



HTTP Request
```http
GET /v5/crypto-loan-common/max-collateral-amount
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| currency | true | string | Collateral coin |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| maxCollateralAmount | string | Max. reduction collateral amount |

---

Request Example

HTTP
 
  
```http
GET /v5/crypto-loan-common/max-collateral-amount?currency=BTC HTTP/1.1
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
    "retMsg": "ok",
    "result": {
        "maxCollateralAmount": "0.08585184"
    },
    "retExtinfo
": {},
    "time": 1728634291554
}
```

