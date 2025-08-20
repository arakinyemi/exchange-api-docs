# Repay
Fully or partially repay a loan. If interest is due, that is paid off first, with the loaned amount being paid off only after due interest.

Permission: "Spot trade"

info

The repaid amount will be deducted from the Funding wallet.

The collateral amount will not be auto returned when you don't fully repay the debt, but you can also adjust collateral amount

HTTP Request
```http
POST /v5/crypto-loan/repay
```

Request Parameters
| Parameter | Required | Type   | Comments      |
|-----------|----------|--------|---------------|
| orderId   | true     | string | Loan order ID |
| amount    | true     | string | Repay amount  |

Response Parameters

| Parameter | Type   | Comments                 |
|-----------|--------|--------------------------|
| repayId   | string | Repayment transaction ID |

---

Request Example

HTTP
```http
POST /v5/crypto-loan/repay HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728629785224
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 61
```
```json
{
    "orderId": "1794267532472646144",
    "amount": "100"
}
```
Response Example
```json
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "repayId": "1794271131730737664"
    },
    "retExtInfo": {},
    "time": 1728629786884
}
```