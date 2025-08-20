# Get Account Borrowable/Collateralizable Limit
Query for the minimum and maximum amounts your account can borrow and how much collateral you can put up.

Permission: "Spot trade"


HTTP Request
```http
GET /v5/crypto-loan/borrowable-collateralisable-number
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| loanCurrency | true | string | Loan coin name |
| collateralCurrency | true | string | Collateral coin name |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| collateralCurrency | string | Collateral coin name |
| loanCurrency | string | Loan coin name |
| maxCollateralAmount | string | Max. limit to mortgage |
| maxLoanAmount | string | Max. limit to borrow |
| minCollateralAmount | string | Min. limit to mortgage |
| minLoanAmount | string | Min. limit to borrow |

---

Request Example

HTTP
 
  
```http
GET /v5/crypto-loan/borrowable-collateralisable-number?loanCurrency=USDT&collateralCurrency=BTC HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728627083198
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "collateralCurrency": "BTC",
        "loanCurrency": "USDT",
        "maxCollateralAmount": "164.957732055526752104",
        "maxLoanAmount": "8000000",
        "minCollateralAmount": "0.000412394330138818",
        "minLoanAmount": "20"
    },
    "retExtinfo
": {},
    "time": 1728627084863
}
```



