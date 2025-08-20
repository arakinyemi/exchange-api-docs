# Get Borrow Orders History
Permission: "Spot trade"

UID rate limit: 5 req / second


HTTP Request
```http
GET /v5/crypto-loan-flexible/borrow-history
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| orderId | false | string | Loan order ID |
| loanCurrency | false | string | Loan coin name |
| limit | false | string | Limit for data size per page. [1, 100]. Default: 10 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > borrowTime | long | The timestamp to borrow |
| > initialLoanAmount | string | Loan amount |
| > loanCurrency | string | Loan coin |
| > orderId | string | Loan order ID |
| > status | integer | Loan order status 1: success; 2: processing; 3: fail |
| nextPageCursor | string | Refer to the cursor request parameter |

---

Request Example

HTTP
 
  
```http
GET /v5/crypto-loan-flexible/borrow-history?limit=2 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752570519918
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
                "borrowTime": 1752569950643,
                "initialLoanAmount": "0.006",
                "loanCurrency": "BTC",
                "orderId": "1364",
                "status": 1
            },
            {
                "borrowTime": 1752569209643,
                "initialLoanAmount": "0.1",
                "loanCurrency": "BTC",
                "orderId": "1363",
                "status": 1
            }
        ],
        "nextPageCursor": "1363"
    },
    "retExtinfo
": {},
    "time": 1752570519414
}
```

