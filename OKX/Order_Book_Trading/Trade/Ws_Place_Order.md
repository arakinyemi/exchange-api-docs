### WS / Place order

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is shared with the Place order REST API endpoints

Request Example

```
{
  "id": "1512",
  "op": "order",
  "args": [
    {
      "side": "buy",
      "instId": "BTC-USDT",
      "tdMode": "cash",
      "ordType": "market",
      "sz": "100"
    }
  ]
}
```

Request Parameters

| Parameter   | Type   | Required | Description                                  |
|-------------|--------|----------|----------------------------------------------|
| id          | String | Yes      | Unique identifier of the message             |
| op          | String | Yes      | Operation: order                            |
| args        | Array  | Yes      | Request parameters                           |

args Structure:

| Parameter  | Type      | Required | Description                                  |
|------------|-----------|----------|----------------------------------------------|
| instId     | String    | Yes      | Instrument ID, e.g. BTC-USDT                 |
| tdMode     | String    | Yes      | Trade mode: cash                             |
| clOrdId    | String    | No       | Client Order ID as assigned by the client     |
| tag        | String    | No       | Order tag                                    |
| side       | String    | Yes      | Order side: buy or sell                      |
| ordType    | String    | Yes      | Order type: market, limit, post_only, fok, ioc|
| sz         | String    | Yes      | Quantity to buy or sell                      |
| px         | String    | Conditional | Order price (see note)                   |
| tgtCcy     | String    | No       | Order quantity unit (base_ccy/quote_ccy)     |
| banAmend   | Boolean   | No       | Disallow system from amending SPOT market order|
| stpId      | String    | No       | Self trade prevention ID (deprecated)        |
| stpMode    | String    | No       | Self trade prevention mode.                  |
| expTime    | String    | No       | Effective deadline (timestamp, ms)            |
| tradeQuoteCcy|String   | No       | Quote currency used for trading (SPOT only)  |

Response Example (success)

```
{
  "id": "1512",
  "op": "order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
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
  "op": "order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "",
      "tag": "",
      "sCode": "5XXXX",
      "sMsg": "not exist"
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
  "op": "order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

Response Parameters

| Parameter   | Type   | Description                                    |
|-------------|--------|------------------------------------------------|
| id          | String | Unique identifier of the message               |
| op          | String | Operation                                      |
| code        | String | Error Code                                     |
| msg         | String | Error Message                                  |
| data        | Array  | Data                                           |
| inTime      | String | Gateway received timestamp (microseconds)      |
| outTime     | String | Gateway response sent timestamp (microseconds) |

data Structure:

| Field    | Type   | Description                                    |
|----------|--------|------------------------------------------------|
| ordId    | String | Order ID                                       |
| clOrdId  | String | Client Order ID as assigned by the client      |
| tag      | String | Order tag                                      |
| sCode    | String | Status code (0 means success)                  |
| sMsg     | String | Event execution success/failure message        |
