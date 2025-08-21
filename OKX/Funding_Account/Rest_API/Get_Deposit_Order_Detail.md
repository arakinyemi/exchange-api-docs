### Get Deposit Order Detail

Get fiat deposit order detail.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Read

HTTP Request
```
GET /api/v5/fiat/deposit
```

Request Example
```
GET /api/v5/fiat/deposit?ordId=024041201450544699
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
        "clientId": ""
    }
  ]
}
```

Response Parameters

| Parameter        | Type   | Description                                                                                   |
|------------------|--------|-----------------------------------------------------------------------------------------------|
| ordId            | String | Order ID                                                                                      |
| clientId         | String | The original request ID associated with the transaction. For deposits, likely an empty string ("") |
| amt              | String | Amount of the transaction                                                                     |
| ccy              | String | The currency of the transaction                                                              |
| fee              | String | The transaction fee                                                                          |
| paymentAcctId    | String | The ID of the payment account used                                                           |
| paymentMethod    | String | Payment method, e.g. `TR_BANKS`                                                              |
| state            | String | The state of the transaction: <br>`completed` <br>`failed` <br>`pending` <br>`canceled` <br>`inqueue` <br>`processing` |
| cTime            | String | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| uTime            | String | The update time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085`  |
