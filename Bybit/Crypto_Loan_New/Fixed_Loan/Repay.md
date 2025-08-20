# Repay
Permission: "Spot trade"

UID rate limit: 1 req / second


HTTP Request
```http
POST /v5/crypto-loan-fixed/fully-repay
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| loanId | false | string | Loan contract ID. Either loanId or loanCurrency needs to be passed |
| loanCurrency | false | string | Loan coin. Either loanId or loanCurrency needs to be passed |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| repayId | string | Repayment transaction ID |

---

Request Example

HTTP
 
  
```http
POST /v5/crypto-loan-fixed/fully-repay HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752656296791
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 50
```

```json
{
    "loanId": "570",
    "loanCurrency": "ETH"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "repayId": "1771"
    },
    "retExtinfo
": {},
    "time": 1752569614549
}
```

