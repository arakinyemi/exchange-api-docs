### Cancel Withdrawal Order

Cancel a pending fiat withdrawal order, currently only applicable to TRY.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Trade

HTTP Request
```
POST /api/v5/fiat/cancel-withdrawal
```

Request Example
```
POST /api/v5/fiat/cancel-withdrawal
{
    "ordId": "124041201450544699"
}
```

Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| ordId      | String | Yes      | Payment Order Id |

Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "ordId": "124041201450544699",
        "state": "canceled"
    }
  ]
}
```

Response Parameters
| Parameter | Type   | Description |
|-----------|--------|-------------|
| ordId     | String | Payment Order ID |
| state     | String | The state of the transaction, e.g. canceled |

---
