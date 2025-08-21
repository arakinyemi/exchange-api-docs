### WS / Cancel multiple orders

Cancel incomplete orders in batches (max 20 orders per request).

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 300 orders per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is determined by number of orders. Shared with the `Cancel multiple orders` REST API endpoints.

Request Example

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "args": [
    {
      "instId": "BTC-USDT",
      "ordId": "2517748157541376"
    },
    {
      "instId": "LTC-USDT",
      "ordId": "2517748155771904"
    }
  ]
}
```

Request Parameters

| Parameter | Type   | Required | Description                                              |
|-----------|--------|----------|----------------------------------------------------------|
| id        | String | Yes      | Unique identifier of the message                         |
| op        | String | Yes      | Operation: batch-cancel-orders                           |
| args      | Array  | Yes      | Request Parameters                                       |

args structure:

| Parameter | Type   | Required    | Description                                             |
|-----------|--------|-------------|---------------------------------------------------------|
| instId    | String | Yes         | Instrument ID                                          |
| ordId     | String | Conditional | Order ID (Either ordId or clOrdId is required; if both, ordId is used) |
| clOrdId   | String | Conditional | Client Order ID as assigned by client                  |

Response Examples

**When All Succeed**

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "2517748157541376",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "2517748155771904",
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

**When Partially Successful**

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "2517748157541376",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "2517748155771904",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "2",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When All Failed**

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "2517748157541376",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "2517748155771904",
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

**When Format Error**

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
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
| sCode     | String | Order status code (0 means success)           |
| sMsg      | String | Order status message                          |

| inTime    | String | Timestamp at WebSocket gateway when request received (microseconds)  |
| outTime   | String | Timestamp at WebSocket gateway when response sent (microseconds)      |

***
