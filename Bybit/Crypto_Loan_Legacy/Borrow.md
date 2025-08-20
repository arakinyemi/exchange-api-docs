# Borrow
Permission: "Spot trade"

info

The loan funds are released to the Funding wallet.
The collateral funds are deducted from the Funding wallet, so make sure you have enough collateral amount in the Funding wallet.

HTTP Request
```http
POST /v5/crypto-loan/borrow
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| loanCurrency | true | string | Loan coin name |
| loanAmount | false | string | Amount to borrow<br> Required when collateral amount is not filled |
| loanTerm | false | string | Loan term<br> flexible term: null or not passed <br> fixed term: 7, 14, 30, 90, 180 days |
| collateralCurrency | true | string | Currency used to mortgage |
| collateralAmount | false | string | Amount to mortgage <br> Required when loan amount is not filled |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| orderId | string | Loan order ID |

---

Request Example

HTTP
 
  
```http
POST /v5/crypto-loan/borrow HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728629356551
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 140
```

```json
{
    "loanCurrency": "USDT",
    "loanAmount": "550",
    "collateralCurrency": "BTC",
    "loanTerm": null,
    "collateralAmount": null
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "orderId": "1794267532472646144"
    },
    "retExtinfo
": {},
    "time": 1728629357820
}
```
