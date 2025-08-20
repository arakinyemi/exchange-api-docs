# Get Flexible loans
Query for your ongoing loans

Permission: "Spot trade"
UID rate limit: 5 req / second


HTTP Request
```http
GET /v5/crypto-loan-flexible/ongoing-coin
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| loanCurrency | false | string | Loan coin name |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > hourlyInterestRate | string | Latest hourly flexible interest rate |
| > loanCurrency | string | Loan coin |
| > totalDebt | string | Unpaid principal and interest |

---

Request Example

HTTP
 
  
```http
GET /v5/crypto-loan-flexible/ongoing-coin?loanCurrency=BTC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752570124973
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "hourlyInterestRate": "0.00000013597784",
                "loanCurrency": "BTC",
                "totalDebt": "0.10100003"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1752570124242
}
```

