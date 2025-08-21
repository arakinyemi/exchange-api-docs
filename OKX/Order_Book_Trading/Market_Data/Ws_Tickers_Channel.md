### WS / Tickers channel

Retrieve the last traded price, bid price, ask price and 24-hour trading volume of instruments.  
Best ask price may be lower than best bid price during the pre-open period.  
The fastest rate is 1 update/100ms. There will be no update if the event is not triggered.  
Events that trigger update: trade, best ask/bid change.

**URL Path**  
`/ws/v5/public`

#### Request Example

```json
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "tickers",
      "instId": "BTC-USDT"
    }
  ]
}
```

#### Request Parameters

| Parameter | Type       | Required | Description                                                                    |
| --------- | ---------- | -------- | ------------------------------------------------------------------------------ |
| id        | String     | No       | Unique identifier of the message (client provided, returned in response)       |
| op        | String     | Yes      | Operation: subscribe, unsubscribe                                              |
| args      | Array Obj. | Yes      | List of subscribed channels                                                    |
| > channel | String     | Yes      | Channel name (`tickers`)                                                       |
| > instId  | String     | Yes      | Instrument ID                                                                 |

#### Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "tickers",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"tickers\", \"instId\" : \"LTC-USD-200327\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response Parameters

| Parameter | Type   | Required | Description                       |
|-----------|--------|----------|---------------------------------|
| id        | String | No       | Unique identifier of the message|
| event     | String | Yes      | Event: subscribe, unsubscribe, error |
| arg       | Object | No       | Subscribed channel info          |
| > channel | String | Yes      | Channel name                    |
| > instId  | String | Yes      | Instrument ID                  |
| code      | String | No       | Error code                    |
| msg       | String | No       | Error message                 |
| connId    | String | Yes      | WebSocket connection ID         |

#### Push Data Example

```json
{
  "arg": {
    "channel": "tickers",
    "instId": "BTC-USDT"
  },
  "data": [
    {
      "instType": "SPOT",
      "instId": "BTC-USDT",
      "last": "9999.99",
      "lastSz": "0.1",
      "askPx": "9999.99",
      "askSz": "11",
      "bidPx": "8888.88",
      "bidSz": "5",
      "open24h": "9000",
      "high24h": "10000",
      "low24h": "8888.88",
      "volCcy24h": "2222",
      "vol24h": "2222",
      "sodUtc0": "2222",
      "sodUtc8": "2222",
      "ts": "1597026383085"
    }
  ]
}
```

#### Push Data Parameters

| Parameter  | Type           | Description                                              |
|------------|----------------|----------------------------------------------------------|
| arg        | Object         | Successfully subscribed channel                           |
| > channel  | String         | Channel name                                             |
| > instId   | String         | Instrument ID                                            |
| data       | Array of Obj.  | Subscribed data                                          |
| > instType | String         | Instrument type                                         |
| > instId   | String         | Instrument ID                                          |
| > last     | String         | Last traded price                                      |
| > lastSz   | String         | Last traded size                                      |
| > askPx    | String         | Best ask price                                        |
| > askSz    | String         | Best ask size                                        |
| > bidPx    | String         | Best bid price                                       |
| > bidSz    | String         | Best bid size                                       |
| > open24h  | String         | Open price in past 24 hours                          |
| > high24h  | String         | Highest price in past 24 hours                       |
| > low24h   | String         | Lowest price in past 24 hours                        |
| > volCcy24h| String         | 24h trading volume (SPOT: quote currency)           |
| > vol24h   | String         | 24h trading volume (SPOT: base currency)            |
| > sodUtc0  | String         | Open price UTC 0                                     |
| > sodUtc8  | String         | Open price UTC 8                                     |
| > ts       | String         | Ticker data timestamp (ms)                           |

***
