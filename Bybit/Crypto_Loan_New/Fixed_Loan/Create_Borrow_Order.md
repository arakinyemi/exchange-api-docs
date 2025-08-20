# Create Borrow Order
Permission: "Spot trade"
UID rate limit: 1 req / second

info

The loan funds are released to the Funding wallet.

The collateral funds are deducted from the Funding wallet, so make sure you have enough collateral amount in the Funding wallet.

HTTP Request
```http
POST /v5/crypto-loan-fixed/borrow
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| orderCurrency | true | string | Currency to borrow |
| orderAmount | true | string | Amount to borrow |
| annualRate | true | string | Customizable annual interest rate, e.g., 0.02 means 2% |
| term | true | string | Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days |
| autoRepay | false | string | Enable Auto-Repay to have assets in your Funding Account automatically repay your loan upon Borrowing order expiration, preventing overdue penalties. Ensure your Funding Account maintains sufficient amount for repayment to avoid automatic repayment failures.<br> "true": enable, default; "false": disable |
| collateralList | false | array`<object>` | Collateral coin list, supports putting up to 100 currency in the array |
| > currency | false | string | Currency used to mortgage |
| > amount | false | string | Amount to mortgage |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| orderId | string | Loan order ID |

---

Request Example

HTTP
 
  
```http
POST /v5/crypto-loan-fixed/borrow HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752633649752
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 208
```

```json
{
    "orderCurrency": "ETH",
    "orderAmount": "1.5",
    "annualRate": "0.022",
    "term": "30",
    "autoRepay": "true",
    "collateralList": {
        "currency": "BTC",
        "amount": "0.1"
    }
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "orderId": "13007"
    },
    "retExtinfo
": {},
    "time": 1752633650147
}
```

