### Get Withdrawal Order Detail

Get fiat withdraw order detail.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Read

HTTP Request
```
GET /api/v5/fiat/withdrawal
```

Request Example
```
GET /api/v5/fiat/withdrawal?ordId=024041201450544699
{
    "ordId": "024041201450544699"
}
```

Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| ordId      | String | Yes      | Order ID |

Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "024041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "100",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": "194a6975e98246538faeb0fab0d502df"
    }
  ]
}
```

Response Parameters
| Parameter      | Type   | Description |
|----------------|--------|-------------|
| ordId          | String | Order ID |
| clientId       | String | The original request ID associated with the transaction |
| ccy            | String | The currency of the transaction |
| amt            | String | Amount of the transaction |
| fee            | String | The transaction fee |
| paymentAcctId  | String | The ID of the payment account used |
| paymentMethod  | String | Payment method, e.g. TR_BANKS |
| state          | String | The state of the transaction (completed, failed, pending, canceled, inqueue, processing) |
| cTime          | String | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| uTime          | String | The update time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |

---
