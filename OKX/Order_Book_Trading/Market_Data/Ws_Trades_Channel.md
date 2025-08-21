### WS / Trades channel

Retrieve recent trades data. Data pushes whenever there is a trade. Each update can aggregate multiple trades.

The message is sent only once per taker order, per filled price. The `count` field represents the number of aggregated matches.

**URL Path**  
`/ws/v5/public`

#### Request Example

```json
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "trades",
      "instId": "BTC-USDT"
    }
  ]
}
```

#### Request Parameters

| Parameter | Type        | Required | Description                                                             |
|-----------|-------------|----------|-------------------------------------------------------------------------|
| id        | String      | No       | Unique identifier of the message (client provided, returned in response)|
| op        | String      | Yes      | Operation: subscribe, unsubscribe                                      |
| args      | Array Obj.  | Yes      | List of subscribed channels                                             |
| > channel | String      | Yes      | Channel name (`trades`)                                                 |
| > instId  | String      | Yes      | Instrument ID                                                          |

#### Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
      "channel": "trades",
      "instId": "BTC-USDT"
  },
  "connId": "a4d3ae55"
}
```

#### Failure Response Example

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"trades\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response Parameters

| Parameter | Type    | Required | Description                  |
|-----------|---------|----------|------------------------------|
| id        | String  | No       | Unique identifier of message |
| event     | String  | Yes      | Event: subscribe, unsubscribe, error          |
| arg       | Object  | No       | Subscribed channel info      |
| > channel | String  | Yes      | Channel name                 |
| > instId  | String  | Yes      | Instrument ID               |
| code      | String  | No       | Error code                  |
| msg       | String  | No       | Error message               |
| connId    | String  | Yes      | WebSocket connection ID     |

#### Push Data Example

```json
{
  "arg": {
    "channel": "trades",
    "instId": "BTC-USDT"
  },
  "data": [
    {
      "instId": "BTC-USDT",
      "tradeId": "130639474",
      "px": "42219.9",
      "sz": "0.12060306",
      "side": "buy",
      "ts": "1630048897897",
      "count": "3",
      "seqId": 1234
    }
  ]
}
```

#### Push Data Parameters

| Parameter  | Type    | Description                                                                                                 |
|------------|---------|-------------------------------------------------------------------------------------------------------------|
| arg        | Object  | Successfully subscribed channel                                                                              |
| > channel  | String  | Channel name                                                                                                |
| > instId   | String  | Instrument ID                                                                                               |
| data       | Array   | Subscribed data                                                                                              |
| > instId   | String  | Instrument ID, e.g. BTC-USDT                                                                                |
| > tradeId  | String  | Last trade ID in the trade aggregation                                                                     |
| > px       | String  | Trade price                                                                                                |
| > sz       | String  | Trade quantity (SPOT unit: base currency; others: contracts)                                               |
| > side     | String  | Trade side of taker (buy/sell)                                                                             |
| > ts       | String  | Filled time, Unix timestamp in milliseconds                                                                |
| > count    | String  | Number of trades aggregated                                                                                  |
| > seqId    | Integer | Sequence ID of the current message                                                                          |

**Aggregation function description:**  
1. The system sends one message per taker order, per filled price. `count` represents the number of aggregated matches.  
2. `tradeId` becomes the last trade ID in the aggregation.  
3. `count=1` means taker order matches one maker order with the specific price.  
4. `count>1` means taker order matches multiple maker orders with the same price (e.g., if `tradeId=123` and `count=3`, message aggregates trades 123, 122, 121).  
5. Use this info to compare with `trades-all` channel data.  
6. Order book and aggregated trades data publish sequentially.  
7. `seqId` may be same for different trade updates occurring simultaneously.

***
