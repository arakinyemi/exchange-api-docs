## Live Subscribing/Unsubscribing to streams

The following data can be sent through the WebSocket instance in order to subscribe/unsubscribe from streams. Examples can be seen below.

The id is used as an identifier to uniquely identify the messages going back and forth. The following formats are accepted:
- 64-bit signed integer
- alphanumeric strings; max length 36
- null

In the response, if the result received is null this means the request sent was a success for non-query requests (e.g. Subscribing/Unsubscribing).

### Subscribe to a stream

**Request**
```json
{
  "method": "SUBSCRIBE",
  "params": [
    "btcusdt@aggTrade",
    "btcusdt@depth"
  ],
  "id": 1
}
```

**Response**
```json
{
  "result": null,
  "id": 1
}
```

### Unsubscribe to a stream

**Request**
```json
{
  "method": "UNSUBSCRIBE",
  "params": [
    "btcusdt@depth"
  ],
  "id": 312
}
```

**Response**
```json
{
  "result": null,
  "id": 312
}
```

### Listing Subscriptions

**Request**
```json
{
  "method": "LIST_SUBSCRIPTIONS",
  "id": 3
}
```

**Response**
```json
{
  "result": [
    "btcusdt@aggTrade"
  ],
  "id": 3
}
```

### Setting Properties

Currently, the only property that can be set is whether combined stream payloads are enabled or not. The combined property is set to false when connecting using `/ws/` ("raw streams") and true when connecting using `/stream/`.

**Request**
```json
{
  "method": "SET_PROPERTY",
  "params": [
    "combined",
    true
  ],
  "id": 5
}
```

**Response**
```json
{
  "result": null,
  "id": 5
}
```

### Retrieving Properties

**Request**
```json
{
  "method": "GET_PROPERTY",
  "params": [
    "combined"
  ],
  "id": 2
}
```

**Response**
```json
{
  "result": true, // Indicates that combined is set to true.
  "id": 2
}
```

### Error Messages

| Error Message | Description |
|---------------|-------------|
| `{"code": 0, "msg": "Unknown property","id": %s}` | Parameter used in the SET_PROPERTY or GET_PROPERTY was invalid |
| `{"code": 1, "msg": "Invalid value type: expected Boolean"}` | Value should only be true or false |
| `{"code": 2, "msg": "Invalid request: property name must be a string"}` | Property name provided was invalid |
| `{"code": 2, "msg": "Invalid request: request ID must be an unsigned integer"}` | Parameter id had to be provided or the value provided in the id parameter is an unsupported type |
| `{"code": 2, "msg": "Invalid request: unknown variant %s, expected one of SUBSCRIBE, UNSUBSCRIBE, LIST_SUBSCRIPTIONS, SET_PROPERTY, GET_PROPERTY at line 1 column 28"}` | Possible typo in the provided method or provided method was neither of the expected values |
| `{"code": 2, "msg": "Invalid request: too many parameters"}` | Unnecessary parameters provided in the data |
| `{"code": 2, "msg": "Invalid request: property name must be a string"}` | Property name was not provided |
| `{"code": 2, "msg": "Invalid request: missing field method at line 1 column 73"}` | method was not provided in the data |
| `{"code":3,"msg":"Invalid JSON: expected value at line %s column %s"}` | JSON data sent has incorrect syntax. |
