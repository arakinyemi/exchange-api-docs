### UI Klines​

```
{  
  "id": "b137468a-fb20-4c06-bd6b-625148eec958",  
  "method": "uiKlines",  
  "params": {  
    "symbol": "BNBBTC",  
    "interval": "1h",  
    "startTime": 1655969280000,  
    "limit": 1  
  }  
}
```

Get klines (candlestick bars) optimized for presentation.

This request is similar to [`klines`](/docs/binance-spot-api-docs/websocket-api/market-data-requests#klines), having the same parameters and response.
`uiKlines` return modified kline data, optimized for presentation of candlestick charts.

**Weight:**
2

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `interval` | ENUM | YES | See [`klines`](/docs/binance-spot-api-docs/websocket-api/market-data-requests#kline-intervals) |
| `startTime` | LONG | NO |  |
| `endTime` | LONG | NO |  |
| `timeZone` | STRING | NO | Default: 0 (UTC) |
| `limit` | INT | NO | Default: 500; Maximum: 1000 |

Notes:

* If `startTime`, `endTime` are not specified, the most recent klines are returned.
* Supported values for `timeZone`:
  * Hours and minutes (e.g. `-1:00`, `05:45`)
  * Only hours (e.g. `0`, `8`, `4`)
  * Accepted range is strictly [-12:00 to +14:00] inclusive
* If `timeZone` provided, kline intervals are interpreted in that timezone instead of UTC.
* Note that `startTime` and `endTime` are always interpreted in UTC, regardless of timeZone.

**Data Source:**
Database

**Response:**

```
{  
  "id": "b137468a-fb20-4c06-bd6b-625148eec958",  
  "status": 200,  
  "result": [  
    [  
      1655971200000,      // Kline open time  
      "0.01086000",       // Open price  
      "0.01086600",       // High price  
      "0.01083600",       // Low price  
      "0.01083800",       // Close price  
      "2290.53800000",    // Volume  
      1655974799999,      // Kline close time  
      "24.85074442",      // Quote asset volume  
      2283,               // Number of trades  
      "1171.64000000",    // Taker buy base asset volume  
      "12.71225884",      // Taker buy quote asset volume  
      "0"                 // Unused field, ignore  
    ]  
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