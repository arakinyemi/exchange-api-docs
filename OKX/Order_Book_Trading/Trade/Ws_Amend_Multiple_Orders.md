### WS / Amend multiple orders

Amend incomplete orders in batch (max 20 orders per request).

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 300 orders per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is shared with the `Amend multiple orders` REST API endpoints.

Request Example

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "args": [
    {
      "instId": "BTC-USDT",
      "ordId": "12345689",
      "newSz": "2"
    },
    {
      "instId": "BTC-USDT",
      "ordId": "12344",
      "newSz": "2"
    }
  ]
}
```

Request Parameters

| Parameter | Type   | Required | Description                                          |
|-----------|--------|----------|------------------------------------------------------|
| id        | String | Yes      | Unique identifier of the message                     |
| op        | String | Yes      | Operation: batch-amend-orders                         |
| args      | Array  | Yes      | Request parameters                                   |

args structure:

| Parameter | Type   | Required    | Description                                           |
|-----------|--------|-------------|-------------------------------------------------------|
| instId    | String | Yes         | Instrument ID                                       |
| cxlOnFail | Boolean| No          | Cancel order automatically on amend failure         |
| ordId     | String | Conditional | Order ID (either ordId or clOrdId required; ordId prioritized) |
| clOrdId   | String | Conditional | Client Order ID                                     |
| reqId     | String | No          | Client Request ID for amendment                     |
| newSz     | String | Conditional | New quantity after amendment (either newSz or newPx required)|
| newPx     | String | Conditional | New price after amendment                           |
| expTime   | String | No          | Request effective deadline (timestamp ms)           |

Response Examples

**When All Succeed**

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "12345689",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "12344",
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

**When All Failed**

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "reqId": "b12344",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "",
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

**When Partially Successful**

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "",
      "reqId": "b12344",
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

**When Format Error**

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

Response Parameters

| Parameter | Type   | Description                                           |
|-----------|--------|-------------------------------------------------------|
| id        | String | Unique identifier of the message                      |
| op        | String | Operation                                             |
| code      | String | Error Code                                            |
| msg       | String | Error message                                         |
| data      | Array  | Data                                                  |

data Structure:

| Parameter | Type   | Description                                           |
|-----------|--------|-------------------------------------------------------|
| ordId     | String | Order ID                                            |
| clOrdId   | String | Client Order ID                                     |
| reqId     | String | Client Request ID for amendment                     |
| sCode     | String | Order status code (0 means success)                   |
| sMsg      | String | Order status message                                |

| inTime    | String | Timestamp at WebSocket gateway when request received (microseconds) |
| outTime   | String | Timestamp at WebSocket gateway when response sent (microseconds)     |

***
