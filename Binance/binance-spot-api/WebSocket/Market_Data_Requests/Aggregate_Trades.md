### Aggregate trades​

```
{  
  "id": "189da436-d4bd-48ca-9f95-9f613d621717",  
  "method": "trades.aggregate",  
  "params": {  
    "symbol": "BNBBTC",  
    "fromId": 50000000,  
    "limit": 1  
  }  
}
```

Get aggregate trades.

An *aggregate trade* (aggtrade) represents one or more individual trades.
Trades that fill at the same time, from the same taker order, with the same price –
those trades are collected into an aggregate trade with total quantity of the individual trades.

If you need access to real-time trading activity, please consider using WebSocket Streams:

* [`<symbol>@aggTrade`](/docs/binance-spot-api-docs/web-socket-streams#aggregate-trade-streams)

If you need historical aggregate trade data,
please consider using data.binance.vision.

**Weight:**
4

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `fromId` | INT | NO | Aggregate trade ID to begin at |
| `startTime` | LONG | NO |  |
| `endTime` | LONG | NO |  |
| `limit` | INT | NO | Default: 500; Maximum: 1000 |

Notes:

* If `fromId` is specified, return aggtrades with aggregate trade ID >= `fromId`.

  Use `fromId` and `limit` to page through all aggtrades.
* If `startTime` and/or `endTime` are specified, aggtrades are filtered by execution time (`T`).

  `fromId` cannot be used together with `startTime` and `endTime`.
* If no condition is specified, the most recent aggregate trades are returned.

**Data Source:**
Database

**Response:**

```
{  
  "id": "189da436-d4bd-48ca-9f95-9f613d621717",  
  "status": 200,  
  "result": [  
    {  
      "a": 50000000,        // Aggregate trade ID  
      "p": "0.00274100",    // Price  
      "q": "57.19000000",   // Quantity  
      "f": 59120167,        // First trade ID  
      "l": 59120170,        // Last trade ID  
      "T": 1565877971222,   // Timestamp  
      "m": true,            // Was the buyer the maker?  
      "M": true             // Was the trade the best price match?  
    }  
  ],  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 2  
    }  
  ]  
}
```