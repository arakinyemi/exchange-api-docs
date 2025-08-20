# Get Completed Loan History
Query for the last 6 months worth of your completed (fully paid off) loans.

Permission: "Spot trade"


HTTP Request
```http
GET /v5/crypto-loan/borrow-history
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| orderId | false | string | Loan order ID |
| loanCurrency | false | string | Loan coin name |
| collateralCurrency | false | string | Collateral coin name |
| limit | false | string | Limit for data size per page. [1, 100]. Default: 10 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > borrowTime | string | The timestamp to borrow |
| > collateralCurrency | string | Collateral coin |
| > expirationTime | string | Loan maturity time, keeps "" for flexible loan |
| > hourlyInterestRate | string | Hourly interest rate<br> Flexible loan, it is real-time interest rate <br> Fixed term loan: it is fixed term interest rate |
| > initialCollateralAmount | string | Initial amount to mortgage |
| > initialLoanAmount | string | Initial loan amount |
| > loanCurrency | string | Loan coin |
| > loanTerm | string | Loan term, 7, 14, 30, 90, 180 days, keep "" for flexible loan |
| > orderId | string | Loan order ID |
| > repaidInterest | string | Total interest repaid |
| > repaidPenaltyInterest | string | Total penalty interest repaid |
| > status | integer | Loan order status 1: fully repaid manually; 2: fully repaid by liquidation |
| nextPageCursor | string | Refer to the cursor request parameter |

---

Request Example

HTTP
 
  
```http
GET /v5/crypto-loan/borrow-history?orderId=1793683005081680384 HTTP/1.1
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
                "borrowTime": "1728546174028",
                "collateralCurrency": "BTC",
                "expirationTime": "1729148399000",
                "hourlyInterestRate": "0.0000010241",
                "initialCollateralAmount": "0.0494727",
                "initialLoanAmount": "1",
                "loanCurrency": "ETH",
                "loanTerm": "7",
                "orderId": "1793569729874260992",
                "repaidInterest": "0.00000515",
                "repaidPenaltyInterest": "0",
                "status": 1
            }
        ],
        "nextPageCursor": ""
    },
    "retExtinfo
": {},
    "time": 1728632014857
}
```

