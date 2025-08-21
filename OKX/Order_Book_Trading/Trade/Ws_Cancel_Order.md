### WS / Cancel order

Cancel an incomplete order

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is shared with the `Cancel order` REST API endpoints  

Request Example

```
{
  "id": "1514",
  "op": "cancel-order",
  "args": [
    {
      "instId": "BTC-USDT",
      "ordId": "2510789768709120"
    }
  ]
}
```

Request Parameters

| Parameter | Type   | Required    | Description                                                      |
|-----------|--------|-------------|------------------------------------------------------------------|
| id        | String | Yes         | Unique identifier of the message                                 |
| op        | String | Yes         | Operation: cancel-order                                          |
| args      | Array  | Yes         | Request Parameters                                               |

args structure:

| Parameter | Type   | Required    | Description                                                      |
|-----------|--------|-------------|------------------------------------------------------------------|
| instId    | String | Yes         | Instrument ID                                                   |
| ordId     | String | Conditional | Order ID (Either ordId or clOrdId is required; if both, ordId is used) |
| clOrdId   | String | Conditional | Client Order ID (as assigned by client)                         |

Successful Response Example

```
{
  "id": "1514",
  "op": "cancel-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
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
  "id": "1514",
  "op": "cancel-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "sCode": "5XXXX",
      "sMsg": "Order not exist"
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
  "id": "1514",
  "op": "cancel-order",
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
| id        | String | Unique identifier of the message               |
| op        | String | Operation                                      |
| code      | String | Error Code                                     |
| msg       | String | Error message                                  |
| data      | Array  | Data                                           |

data Structure:

| Parameter | Type   | Description                                   |
|-----------|--------|-----------------------------------------------|
| ordId     | String | Order ID                                      |
| clOrdId   | String | Client Order ID                               |
| sCode     | String | Order status code (0 means success)           |
| sMsg      | String | Order status message                           |

| inTime    | String | Timestamp at WebSocket gateway when request received (microseconds)   |
| outTime   | String | Timestamp at WebSocket gateway when response sent (microseconds)      |

> Cancel order returns with sCode equal to 0, meaning your cancellation request has been accepted by the system server. The result is subject to the state pushed by the order channel or the get order state.

***
