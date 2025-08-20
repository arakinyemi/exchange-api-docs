# Borrow
Permission: "Spot trade"
UID rate limit: 1 req / second

info

The loan funds are released to the Funding wallet.

The collateral funds are deducted from the Funding wallet, so make sure you have enough collateral amount in the Funding wallet.

HTTP Request
```http
POST /v5/crypto-loan-flexible/borrow
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| loanCurrency | true | string | Loan coin name |
| loanAmount | true | string | Amount to borrow |
| collateralList | false | array`<object>` | Collateral coin list, supports putting up to 100 currency in the array |
| > collateralCurrency | false | string | Currency used to mortgage |
| > collateralAmount | false | string | Amount to mortgage |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| orderId | string | Loan order ID |

---

Request Example

HTTP
 
  
```http
POST /v5/crypto-loan-flexible/borrow HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752569210041
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 244
```

```json
{
    "loanCurrency": "BTC",
    "loanAmount": "0.1",
    "collateralList": [
        {
            "currency": "USDT",
            "amount": "1000"
        },
        {
            "currency": "ETH",
            "amount": "1"
        }
    ]
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "orderId": "1363"
    },
    "retExtinfo
": {},
    "time": 1752569209682
}
```

