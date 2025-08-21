### WS / Place multiple orders

Place orders in a batch. Maximum 20 orders/request.  
URL Path  
`/ws/v5/private` (required login)

Rate Limit: 300 orders per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is determined by number of orders. If one order, consumes rate limit of Place order. Shared with Place multiple orders REST API.

Request Example

```
{
  "id": "1513",
  "op": "batch-orders",
  "args": [
    {
      "side": "buy",
      "instId": "BTC-USDT",
      "tdMode": "cash",
      "ordType": "market",
      "sz": "100"
    },
    {
      "side": "buy",
      "instId": "LTC-USDT",
      "tdMode": "cash",
      "ordType": "market",
      "sz": "1"
    }
  ]
}
```

Request Parameters

| Parameter  | Type   | Required | Description                              |
|------------|--------|----------|------------------------------------------|
| id         | String | Yes      | Unique identifier of the message         |
| op         | String | Yes      | Operation: batch-orders                  |
| args       | Array  | Yes      | Request Parameters                       |

args Structure:

| Field     | Type     | Required | Description                              |
|-----------|----------|----------|------------------------------------------|
| instId    | String   | Yes      | Instrument ID                            |
| tdMode    | String   | Yes      | Trade mode                               |
| clOrdId   | String   | No       | Client Order ID                          |
| tag       | String   | No       | Order tag                                |
| side      | String   | Yes      | Order side: buy or sell                  |
| ordType   | String   | Yes      | Order type (market, limit, post_only, fok, ioc)|
| sz        | String   | Yes      | Quantity to buy or sell                  |
| px        | String   | Conditional | Order price (see note)                |
| tgtCcy    | String   | No       | Order quantity unit (base_ccy/quote_ccy) |
| banAmend  | Boolean  | No       | Disallow system amending SPOT market order|
| stpId     | String   | No       | Self trade prevention ID (deprecated)    |
| stpMode   | String   | No       | Self trade prevention mode               |
| expTime   | String   | No       | Effective deadline (timestamp, ms)        |
| tradeQuoteCcy|String | No       | Quote currency for trading (SPOT only)   |

Response Examples

**When All Succeed**

```
{
  "id": "1513",
  "op": "batch-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "",
      "ordId": "12344",
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

**When Partially Successful**

```
{
  "id": "1513",
  "op": "batch-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "",
      "ordId": "",
      "tag": "",
      "sCode": "5XXXX",
      "sMsg": "Insufficient margin"
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
  "id": "1513",
  "op": "batch-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "",
      "tag": "",
      "sCode": "5XXXX",
      "sMsg": "Insufficient margin"
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "",
      "tag": "",
      "sCode": "5XXXX",
      "sMsg": "Insufficient margin"
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
  "id": "1513",
  "op": "batch-orders",
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





Here is your API documentation content from the attached file, structured with proper Markdown tables for the tabular data parts. The text content remains unchanged:

***
