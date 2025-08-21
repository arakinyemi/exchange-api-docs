### Get Withdrawal Order History

Get fiat withdrawal order history.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Read

HTTP Request
```
GET /api/v5/fiat/withdrawal-order-history
```

Request Example
```
GET /api/v5/fiat/withdrawal-order-history
```

Request Parameters
| Parameters    | Types  | Required | Description |
|---------------|--------|----------|-------------|
| ccy           | String | No       | Fiat currency, ISO-4217 3 digit currency code, e.g. TRY |
| paymentMethod | String | No       | Payment Method (TR_BANKS, PIX, SEPA) |
| state         | String | No       | State of the order (completed, failed, pending, canceled, inqueue, processing) |
| after         | String | No       | Filter with a begin timestamp. Unix timestamp format in milliseconds (inclusive), e.g. 1597026383085 |
| before        | String | No       | Filter with an end timestamp. Unix timestamp format in milliseconds (inclusive), e.g. 1597026383085 |
| limit         | String | No       | Number of results per request. Maximum and default is 100 |
| tag           | String | No       | Order tag. Applicable to broker user. If the convert trading used tag, this parameter is also required. |

Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "124041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "10000",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": "194a6975e98246538faeb0fab0d502df"
    },
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "124041201450544690",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "5000",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": "164a6975e48946538faeb0fab0d414fg"
    }
  ]
}
```

Response Parameters
| Parameter      | Type   | Description |
|----------------|--------|-------------|
| ordId          | String | Unique Order Id |
| clientId       | String | Client Id of the transaction |
| amt            | String | Final amount of the transaction |
| ccy            | String | Currency of the transaction |
| fee            | String | Transaction fee |
| paymentAcctId  | String | ID of the payment account used |
| paymentMethod  | String | Payment method type |
| state          | String | State of the transaction (completed, failed, pending, canceled, inqueue, processing) |
| cTime          | String | Creation time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| uTime          | String | Update time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |

---
