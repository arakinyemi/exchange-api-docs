### WS / Amend order

Amend an incomplete order.

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is shared with the `Amend order` REST API endpoints

Request Example

```
{
  "id": "1512",
  "op": "amend-order",
  "args": [
    {
      "instId": "BTC-USDT",
      "ordId": "2510789768709120",
      "newSz": "2"
    }
  ]
}
```

Request Parameters

| Parameter | Type   | Required    | Description                                            |
|-----------|--------|-------------|--------------------------------------------------------|
| id        | String | Yes         | Unique identifier of the message                       |
| op        | String | Yes         | Operation: amend-order                                 |
| args      | Array  | Yes         | Request Parameters                                     |

args structure:

| Parameter | Type   | Required    | Description                                              |
|-----------|--------|-------------|----------------------------------------------------------|
| instId    | String | Yes         | Instrument ID                                           |
| cxlOnFail | Boolean| No          | Cancel order automatically on amend failure            |
| ordId     | String | Conditional | Order ID (either ordId or clOrdId required; ordId prioritized)|
| clOrdId   | String | Conditional | Client Order ID                                         |
| reqId     | String | No          | Client Request ID for amendment                         |
| newSz     | String | Conditional | New quantity after amendment (either newSz or newPx required)|
| newPx     | String | Conditional | New price after amendment                               |
| expTime   | String | No          | Request effective deadline (Unix timestamp in ms)       |

Successful Response Example

```
{
  "id": "1512",
  "op": "amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

Failure Response Example

```
{
  "id": "1512",
  "op": "amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "reqId": "b12344",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

Response Example When Format Error

```
{
  "id": "1512",
  "op": "amend-order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

Response Parameters

| Parameter | Type   | Description                                   |
|-----------|--------|-----------------------------------------------|
| id        | String | Unique identifier of the message              |
| op        | String | Operation                                     |
| code      | String | Error Code                                    |
| msg       | String | Error message                                 |
| data      | Array  | Data                                          |

data Structure:

| Parameter | Type   | Description                                   |
|-----------|--------|-----------------------------------------------|
| ordId     | String | Order ID                                     |
| clOrdId   | String | Client Order ID                              |
| reqId     | String | Client Request ID                            |
| sCode     | String | Order status code (0 means success)           |
| sMsg      | String | Order status message                          |

| inTime    | String | Timestamp at WebSocket gateway when request received (microseconds)  |
| outTime   | String | Timestamp at WebSocket gateway when response sent (microseconds)      |

> Note: If the new quantity of a partially-filled order is less than or equal to the filled quantity, the order status changes to "filled".
