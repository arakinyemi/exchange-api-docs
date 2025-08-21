### WS / Order book channel

Retrieve order book data. Best ask price may be lower than the best bid price during the pre-open period.

Use channels:
- `books`: 400 depth levels, initial full snapshot, incremental data every 100 ms
- `books5`: 5 depth levels snapshot, every 100 ms if changes
- `bbo-tbt`: 1 depth level snapshot, every 10 ms if changes
- `books-l2-tbt`: 400 depth levels, initial full snapshot, incremental data every 10 ms
- `books50-l2-tbt`: 50 depth levels, initial full snapshot, incremental data every 10 ms

The push sequence within same connection and symbols:  
`bbo-tbt -> books-l2-tbt -> books50-l2-tbt -> books -> books5`  

**Restrictions:**  
- Can't subscribe simultaneously to `books-l2-tbt` and `books50-l2-tbt`/`books` for the same symbol  
- VIP levels required:  
  - VIP5+ for `books-l2-tbt` (400 depth)  
  - VIP4+ for `books50-l2-tbt` (50 depth)  
- Identity verification required (Login)

**URL Path**  
`/ws/v5/public`

#### Request Example

```json
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "books",
      "instId": "BTC-USDT"
    }
  ]
}
```

#### Request Parameters

| Parameter | Type       | Required | Description                                           |
|-----------|------------|----------|-----------------------------------------------------|
| id        | String     | No       | Unique identifier of message (client provided)      |
| op        | String     | Yes      | Operation: subscribe, unsubscribe                   |
| args      | Array Obj. | Yes      | List of subscribed channels                          |
| > channel | String     | Yes      | Channel name (`books`, `books5`, `bbo-tbt`, etc.)  |
| > instId  | String     | Yes      | Instrument ID                                       |

#### Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "books",
    "instId": "BTC-USDT"
  },
  "connId": "a4d3ae55"
}
```

#### Failure Example

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"books\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response Parameters

| Parameter   | Type   | Required | Description                             |
|-------------|--------|----------|---------------------------------------|
| id          | String | No       | Unique identifier of the message      |
| event       | String | Yes      | Event (subscribe, unsubscribe, error)|
| arg         | Object | No       | Subscribed channel info                |
| > channel   | String | Yes      | Channel name                          |
| > instId    | String | Yes      | Instrument ID                        |
| msg         | String | No       | Error message                        |
| code        | String | No       | Error code                          |
| connId      | String | Yes      | WebSocket connection ID               |

#### Push Data Example: Full Snapshot

```json
{
  "arg": {
    "channel": "books",
    "instId": "BTC-USDT"
  },
  "action": "snapshot",
  "data": [
    {
      "asks": [
        ["8476.98", "415", "0", "13"],
        ["8477", "7", "0", "2"],
        ["8477.34", "85", "0", "1"],
        ["8477.56", "1", "0", "1"],
        ["8505.84", "8", "0", "1"],
        ["8506.37", "85", "0", "1"],
        ["8506.49", "2", "0", "1"],
        ["8506.96", "100", "0", "2"]
      ],
      "bids": [
        ["8476.97", "256", "0", "12"],
        ["8475.55", "101", "0", "1"],
        ["8475.54", "100", "0", "1"],
        ["8475.3", "1", "0", "1"],
        ["8447.32", "6", "0", "1"],
        ["8447.02", "246", "0", "1"],
        ["8446.83", "24", "0", "1"],
        ["8446", "95", "0", "3"]
      ],
      "ts": "1597026383085",
      "checksum": -855196043,
      "prevSeqId": -1,
      "seqId": 123456
    }
  ]
}
```

#### Push Data Example: Incremental Data

```json
{
  "arg": {
    "channel": "books",
    "instId": "BTC-USDT"
  },
  "action": "update",
  "data": [
    {
      "asks": [
        ["8476.98", "415", "0", "13"],
        ["8477", "7", "0", "2"],
        ["8477.34", "85", "0", "1"],
        ["8477.56", "1", "0", "1"],
        ["8505.84", "8", "0", "1"],
        ["8506.37", "85", "0", "1"],
        ["8506.49", "2", "0", "1"],
        ["8506.96", "100", "0", "2"]
      ],
      "bids": [
        ["8476.97", "256", "0", "12"],
        ["8475.55", "101", "0", "1"],
        ["8475.54", "100", "0", "1"],
        ["8475.3", "1", "0", "1"],
        ["8447.32", "6", "0", "1"],
        ["8447.02", "246", "0", "1"],
        ["8446.83", "24", "0", "1"],
        ["8446", "95", "0", "3"]
      ],
      "ts": "1597026383085",
      "checksum": -855196043,
      "prevSeqId": 123456,
      "seqId": 123457
    }
  ]
}
```

#### Push Data Parameters

| Parameter  | Type    | Description                                                    |
|------------|---------|----------------------------------------------------------------|
| arg        | Object  | Successfully subscribed channel                                 |
| > channel  | String  | Channel name                                                  |
| > instId   | String  | Instrument ID                                                |
| action     | String  | Push data action: `snapshot` (full) or `update` (incremental) |
| data       | Array   | Subscribed data                                               |
| > asks     | Array of Arrays | Order book on sell side                                 |
| > bids     | Array of Arrays | Order book on buy side                                  |
| > ts       | String  | Order book generation time, Unix timestamp in ms            |
| > checksum | Integer | Checksum (see below)                                         |
| > prevSeqId| Integer | Sequence ID of last sent message (books channels only)        |
| > seqId    | Integer | Sequence ID of current message (books channels only)           |

An example of asks and bids values: `["411.8", "10", "0", "4"]`
- `"411.8"` depth price  
- `"10"` quantity at price (contracts for derivatives or base currency for Spot/Margin)  
- `"0"` deprecated, always `"0"`  
- `"4"` number of orders at price

> If subscribing to many 50 or 400 depth level channels, use multiple websocket connections each with less than 30 channels.  
> Order book updates occur roughly once per second during the call auction.

***

#### Sequence ID

- `seqId` is market data sequence ID. The set of sequence IDs per channel is the same across websocket connections for the same instrument.  
- `prevSeqId` matches with the previous message’s `seqId`. Usually, `seqId` > `prevSeqId`. Minimum `seqId` is 0; for snapshots, `prevSeqId` = -1.  

**Exceptions:**  
1. If no depth updates for ~60 seconds:  
   - For snapshot update channels: latest snapshot sent.   
   - For incremental data: message with empty asks and bids sent to indicate connection alive; `seqId` same as last message, `prevSeqId` equals `seqId`.  
2. Sequence may reset after maintenance, and then users will receive incremental message with smaller `seqId` than `prevSeqId`. Sequence resumes normally afterward.

**Example:**  
- Snapshot: `prevSeqId = -1`, `seqId = 10`  
- Incremental update 1: `prevSeqId = 10`, `seqId = 15`  
- Incremental update 2 (no change): `prevSeqId = 15`, `seqId = 15`  
- Incremental update 3 (reset): `prevSeqId = 15`, `seqId = 3`  
- Incremental update 4: `prevSeqId = 3`, `seqId = 5`

***

#### Checksum

Helps users verify accuracy of depth data.

**Merging incremental into full data:**  
- After subscribing to incremental order book data, first receive initial full snapshot.  
- On incremental updates, update full snapshot:  
  - If price exists, compare size; if size = 0, delete depth; if size changed, replace.  
  - If price not found, insert maintaining sorted order (bids descending, asks ascending).

**Calculating checksum:**  
- Use first 25 asks and bids from full snapshot to form a string, joining price and size with colons, alternating bid and ask pairs.  
- Compute CRC32 (32-bit signed integer) of this string.

***

#### Examples

1. More than 25 levels bids and asks (only 2 levels shown):

```json
{
  "bids": [
    ["3366.1", "7", "0", "3"],
    ["3366", "6", "3", "4"]
  ],
  "asks": [
    ["3366.8", "9", "10", "3"],
    ["3368", "8", "3", "4"]
  ]
}
```

Check string: `"3366.1:7:3366.8:9:3366:6:3368:8"`

2. Less than 25 levels bids or asks:

```json
{
  "bids": [
    ["3366.1", "7", "0", "3"]
  ],
  "asks": [
    ["3366.8", "9", "10", "3"],
    ["3368", "8", "3", "4"],
    ["3372", "8", "3", "4"]
  ]
}
```

Check string: `"3366.1:7:3366.8:9:3368:8:3372:8"`

***

#### Push Data Example of bbo-tbt channel

```json
{
  "arg": {
    "channel": "bbo-tbt",
    "instId": "BCH-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        [
          "111.06","55154","0","2"
        ]
      ],
      "bids": [
        [
          "111.05","57745","0","2"
        ]
      ],
      "ts": "1670324386802",
      "seqId": 363996337
    }
  ]
}
```

***

#### Push Data Example of books5 channel

```json
{
  "arg": {
    "channel": "books5",
    "instId": "BCH-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        ["111.06","55154","0","2"],
        ["111.07","53276","0","2"],
        ["111.08","72435","0","2"],
        ["111.09","70312","0","2"],
        ["111.1","67272","0","2"]
      ],
      "bids": [
        ["111.05","57745","0","2"],
        ["111.04","57109","0","2"],
        ["111.03","69563","0","2"],
        ["111.02","71248","0","2"],
        ["111.01","65090","0","2"]
      ],
      "instId": "BCH-USDT-SWAP",
      "ts": "1670324386802",
      "seqId": 363996337
    }
  ]
}
```
