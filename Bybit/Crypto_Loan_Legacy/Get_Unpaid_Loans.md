# Get Unpaid Loans
Query for your ongoing loans.

Permission: "Spot trade"


HTTP Request
```http
GET /v5/crypto-loan/ongoing-orders
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| orderId | false | string | Loan order ID |
| loanCurrency | false | string | Loan coin name |
| collateralCurrency | false | string | Collateral coin name |
| loanTermType | false | string |1: fixed term, when query this type, loanTerm must be filled <br> 2: flexible term <br> By default, query all types |
| loanTerm | false | string | 7, 14, 30, 90, 180 days, working when loanTermType=1 |
| limit | false | string | Limit for data size per page. [1, 100]. Default: 10 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > collateralAmount | string | Collateral amount |
| > collateralCurrency | string | Collateral coin |
| > currentLTV | string | Current LTV |
| > expirationTime | string | Loan maturity time, keeps "" for flexible loan |
| > hourlyInterestRate | string | Hourly interest rate <br> Flexible loan, it is real-time interest rate <br> Fixed term loan: it is fixed term interest rate |
| > loanCurrency | string | Loan coin |
| > loanTerm | string | Loan term, 7, 14, 30, 90, 180 days, keep "" for flexible loan |
| > orderId | string | Loan order ID |
| > residualInterest | string | Unpaid interest |
| > residualPenaltyInterest | string | Unpaid penalty interest |
| > totalDebt | string | Unpaid principal |
| nextPageCursor | string | Refer to the cursor request parameter |

---

Request Example

HTTP
 
  
```http
GET /v5/crypto-loan/ongoing-orders?orderId=1793683005081680384 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728630979731
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "list": [
            {
                "collateralAmount": "0.0964687",
                "collateralCurrency": "BTC",
                "currentLTV": "0.4161",
                "expirationTime": "1731149999000",
                "hourlyInterestRate": "0.0000010633",
                "loanCurrency": "USDT",
                "loanTerm": "30",
                "orderId": "1793683005081680384",
                "residualInterest": "0.04016",
                "residualPenaltyInterest": "0",
                "totalDebt": "1888.005198"
            }
        ],
        "nextPageCursor": ""
    },
    "retExtinfo
": {},
    "time": 1728630980861
}
```

