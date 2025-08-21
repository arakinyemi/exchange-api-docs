### WS / All trades channel

Retrieve recent trades data. Data pushes whenever there is a trade. Every update contains only one trade.

**URL Path**  
`/ws/v5/business`

#### Request Example

```json
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "trades-all",
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
| > channel | String      | Yes      | Channel name (`trades-all`)                                             |
| > instId  | String      | Yes      | Instrument ID                                                          |

#### Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
      "channel": "trades-all",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"trades-all\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response Parameters

| Parameter | Type    | Required | Description                           |
|-----------|---------|----------|-------------------------------------|
| id        | String  | No       | Unique identifier of message        |
| event     | String  | Yes      | Event: subscribe, unsubscribe, error |
| arg       | Object  | No       | Subscribed channel info             |
| > channel | String  | Yes      | Channel name                       |
| > instId  | String  | Yes      | Instrument ID                      |
| code      | String  | No       | Error code                        |
| msg       | String  | No       | Error message                     |
| connId    | String  | Yes      | WebSocket connection ID           |

#### Push Data Example

```json
{
  "arg": {
    "channel": "trades-all",
    "instId": "BTC-USDT"
  },
  "data": [
    {
      "instId": "BTC-USDT",
      "tradeId": "130639474",
      "px": "42219.9",
      "sz": "0.12060306",
      "side": "buy",
      "ts": "1630048897897"
    }
  ]
}
```

#### Push Data Parameters

| Parameter | Type     | Description                                                                                      |
|-----------|----------|------------------------------------------------------------------------------------------------|
| arg       | Object   | Successfully subscribed channel                                                                 |
| > channel | String   | Channel name                                                                                   |
| > instId  | String   | Instrument ID                                                                                |
| data      | Array    | Subscribed data                                                                              |
| > instId  | String   | Instrument ID, e.g. BTC-USDT                                                                  |
| > tradeId | String   | Trade ID                                                                                    |
| > px      | String   | Trade price                                                                               |
| > sz      | String   | Trade quantity (SPOT: base currency; FUTURES/SWAP/OPTION: contracts)                         |
| > side    | String   | Trade direction (buy/sell)                                                            |
| > ts      | String   | Filled time, Unix timestamp in milliseconds                                                |

***
