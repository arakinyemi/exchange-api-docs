### Create Withdrawal Order

Initiate a fiat withdrawal request (Authenticated endpoint, Only for API keys with "Withdrawal" access).

> **Note:** Only supported withdrawal of assets from funding account.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Withdraw

HTTP Request
```
POST /api/v5/fiat/create-withdrawal
```

Request Example
```
POST /api/v5/fiat/create-withdrawal
{
    "paymentAcctId": "412323",
    "ccy": "TRY",
    "amt": "10000",
    "paymentMethod": "TR_BANKS",
    "clientId": "194a6975e98246538faeb0fab0d502df"
}
```

Request Parameters
| Parameters     | Type   | Required | Description |
|----------------|--------|----------|-------------|
| paymentAcctId  | String | Yes      | Payment account id to withdraw to, retrieved from get withdrawal payment methods API |
| ccy            | String | Yes      | Currency for withdrawal, must match currency allowed for paymentMethod |
| amt            | String | Yes      | Requested withdrawal amount before fees. Has to be less than or equal to 2 decimal points double |
| paymentMethod  | String | Yes      | Payment method to use for withdrawal (TR_BANKS, PIX, SEPA) |
| clientId       | String | Yes      | Client-supplied ID, A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. e.g. 194a6975e98246538faeb0fab0d502df |
| clTReqId       | String | No       | Client Order ID as assigned by the client. A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag            | String | No       | Order tag. Applicable to broker user |

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
        "clientId": ""
    }
  ]
}
```

Response Parameters
| Parameter      | Type   | Description |
|----------------|--------|-------------|
| ordId          | String | Order ID |
| clientId       | String | The original request ID associated with the transaction. If it's a deposit, it's most likely an empty string (""). |
| amt            | String | Amount of the transaction |
| ccy            | String | The currency of the transaction |
| fee            | String | The transaction fee |
| paymentAcctId  | String | The ID of the payment account used |
| paymentMethod  | String | Payment method, e.g. TR_BANKS |
| state          | String | The state of the transaction (completed, failed, pending, canceled, inqueue, processing) |
| cTime          | String | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| uTime          | String | The update time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
```
        "uTime": "1707429385000",
        "ordId": "124041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "fee": "0",
        "amt": "100",
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
| ordId          | String | The unique order Id |
| clientId       | String | The client ID associated with the transaction |
| amt            | String | The requested amount for the transaction |
| ccy            | String | The currency of the transaction |
| fee            | String | The transaction fee |
| paymentAcctId  | String | The Id of the payment account used |
| paymentMethod  | String | Payment Method (TR_BANKS, PIX, SEPA) |
| state          | String | The State of the transaction (processing, completed) |
| cTime          | String | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| uTime          | String | The update time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |

---
