### WS / Candlesticks channel

Retrieve candlestick data of an instrument.  
Push frequency can be as fast as 1 second.

**URL Path**  
`/ws/v5/business`

#### Request Example

```json
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "candle1D",
      "instId": "BTC-USDT"
    }
  ]
}
```

#### Request Parameters

| Parameter | Type       | Required | Description                                         |
|-----------|------------|----------|---------------------------------------------------|
| id        | String     | No       | Unique identifier of the message (client provided, returned in response) |
| op        | String     | Yes      | Operation: subscribe, unsubscribe                 |
| args      | Array Obj. | Yes      | List of subscribed channels                        |
| > channel | String     | Yes      | Channel name, e.g. candle3M, candle1M, candle1D etc. |
| > instId  | String     | Yes      | Instrument ID                                     |

#### Successful Response Example

```json
{
  "op": "subscribe",
  "event": "subscribe",
  "arg": {
    "channel": "candle1D",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"candle1D\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response Parameters

| Parameter | Type    | Required | Description                             |
|-----------|---------|----------|---------------------------------------|
| id        | String  | No       | Unique identifier of the message      |
| event     | String  | Yes      | Event: subscribe, unsubscribe, error  |
| arg       | Object  | No       | Subscribed channel info                |
| > channel | String  | Yes      | Channel name                          |
| > instId  | String  | Yes      | Instrument ID                        |
| code      | String  | No       | Error code                           |
| msg       | String  | No       | Error message                        |
| connId    | String  | Yes      | WebSocket connection ID               |

#### Push Data Example

```json
{
  "arg": {
    "channel": "candle1D",
    "instId": "BTC-USDT"
  },
  "data": [
    [
      "1597026383085",
      "8533.02",
      "8553.74",
      "8527.17",
      "8548.26",
      "45247",
      "529.5858061",
      "5529.5858061",
      "0"
    ]
  ]
}
```

#### Push Data Parameters

| Parameter   | Type           | Description                                                                                       |
|-------------|----------------|-------------------------------------------------------------------------------------------------|
| arg         | Object         | Successfully subscribed channel                                                                  |
| > channel   | String         | Channel name                                                                                    |
| > instId    | String         | Instrument ID                                                                                   |
| data        | Array of Array | Subscribed data                                                                                  |
| > ts        | String         | Opening time of candlestick, Unix timestamp in ms                                              |
| > o         | String         | Open price                                                                                    |
| > h         | String         | Highest price                                                                                  |
| > l         | String         | Lowest price                                                                                   |
| > c         | String         | Close price                                                                                   |
| > vol       | String         | Trading volume (SPOT: base currency)                                                          |
| > volCcy    | String         | Trading volume (SPOT: quote currency)                                                         |
| > volCcyQuote | String       | Trading volume (quantity in quote currency, e.g. USDT for BTC-USDT)                            |
| > confirm   | String         | State of candlesticks: 0 (uncompleted), 1 (completed)                                          |

***
