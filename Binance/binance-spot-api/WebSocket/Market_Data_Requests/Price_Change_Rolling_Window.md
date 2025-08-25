### Rolling window price change statistics​

```
{  
  "id": "f4b3b507-c8f2-442a-81a6-b2f12daa030f",  
  "method": "ticker",  
  "params": {  
    "symbols": [  
      "BNBBTC",  
      "BTCUSDT"  
    ],  
    "windowSize": "7d"  
  }  
}
```

Get rolling window price change statistics with a custom window.

This request is similar to [`ticker.24hr`](/docs/binance-spot-api-docs/websocket-api/market-data-requests#24hr-ticker-price-change-statistics),
but statistics are computed on demand using the arbitrary window you specify.

**Note:** Window size precision is limited to 1 minute.
While the `closeTime` is the current time of the request, `openTime` always start on a minute boundary.
As such, the effective window might be up to 59999 ms wider than the requested `windowSize`.

Window computation example

For example, a request for `"windowSize": "7d"` might result in the following window:

```
"openTime": 1659580020000,  
"closeTime": 1660184865291,
```

Time of the request – `closeTime` – is 1660184865291 (August 11, 2022 02:27:45.291).
Requested window size should put the `openTime` 7 days before that – August 4, 02:27:45.291 –
but due to limited precision it ends up a bit earlier: 1659580020000 (August 4, 2022 02:27:00),
exactly at the start of a minute.

If you need to continuously monitor trading statistics, please consider using WebSocket Streams:

* [`<symbol>@ticker_<window_size>`](/docs/binance-spot-api-docs/web-socket-streams#individual-symbol-rolling-window-statistics-streams) or [`!ticker_<window-size>@arr`](/docs/binance-spot-api-docs/web-socket-streams#all-market-rolling-window-statistics-streams)

**Weight:**
Adjusted based on the number of requested symbols:

| Symbols | Weight |
| --- | --- |
| 1–50 | 4 per symbol |
| 51–100 | 200 |

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES | Query ticker of a single symbol |
| `symbols` | ARRAY of STRING | Query ticker for multiple symbols |
| `type` | ENUM | NO | Ticker type: `FULL` (default) or `MINI` |
| `windowSize` | ENUM | NO | Default `1d` |

Supported window sizes:

| Unit | `windowSize` value |
| --- | --- |
| minutes | `1m`, `2m` ... `59m` |
| hours | `1h`, `2h` ... `23h` |
| days | `1d`, `2d` ... `7d` |

Notes:

* Either `symbol` or `symbols` must be specified.
* Maximum number of symbols in one request: 200.
* Window size units cannot be combined.
  E.g., `1d 2h` is not supported.

**Data Source:**
Database

**Response:**

`FULL` type, for a single symbol:

```
{  
  "id": "f4b3b507-c8f2-442a-81a6-b2f12daa030f",  
  "status": 200,  
  "result": {  
    "symbol": "BNBBTC",  
    "priceChange": "0.00061500",  
    "priceChangePercent": "4.735",  
    "weightedAvgPrice": "0.01368242",  
    "openPrice": "0.01298900",  
    "highPrice": "0.01418800",  
    "lowPrice": "0.01296000",  
    "lastPrice": "0.01360400",  
    "volume": "587179.23900000",  
    "quoteVolume": "8034.03382165",  
    "openTime": 1659580020000,  
    "closeTime": 1660184865291,  
    "firstId": 192977765,       // First trade ID  
    "lastId": 195365758,        // Last trade ID  
    "count": 2387994            // Number of trades  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 4  
    }  
  ]  
}
```

`MINI` type, for a single symbol:

```
{  
  "id": "bdb7c503-542c-495c-b797-4d2ee2e91173",  
  "status": 200,  
  "result": {  
    "symbol": "BNBBTC",  
    "openPrice": "0.01298900",  
    "highPrice": "0.01418800",  
    "lowPrice": "0.01296000",  
    "lastPrice": "0.01360400",  
    "volume": "587179.23900000",  
    "quoteVolume": "8034.03382165",  
    "openTime": 1659580020000,  
    "closeTime": 1660184865291,  
    "firstId": 192977765,       // First trade ID  
    "lastId": 195365758,        // Last trade ID  
    "count": 2387994            // Number of trades  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 4  
    }  
  ]  
}
```

If more than one symbol is requested, response returns an array:

```
{  
  "id": "f4b3b507-c8f2-442a-81a6-b2f12daa030f",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BNBBTC",  
      "priceChange": "0.00061500",  
      "priceChangePercent": "4.735",  
      "weightedAvgPrice": "0.01368242",  
      "openPrice": "0.01298900",  
      "highPrice": "0.01418800",  
      "lowPrice": "0.01296000",  
      "lastPrice": "0.01360400",  
      "volume": "587169.48600000",  
      "quoteVolume": "8033.90114517",  
      "openTime": 1659580020000,  
      "closeTime": 1660184820927,  
      "firstId": 192977765,  
      "lastId": 195365700,  
      "count": 2387936  
    },  
    {  
      "symbol": "BTCUSDT",  
      "priceChange": "1182.92000000",  
      "priceChangePercent": "5.113",  
      "weightedAvgPrice": "23349.27074846",  
      "openPrice": "23135.33000000",  
      "highPrice": "24491.22000000",  
      "lowPrice": "22400.00000000",  
      "lastPrice": "24318.25000000",  
      "volume": "1039498.10978000",  
      "quoteVolume": "24271522807.76838630",  
      "openTime": 1659580020000,  
      "closeTime": 1660184820927,  
      "firstId": 1568787779,  
      "lastId": 1604337406,  
      "count": 35549628  
    }  
  ],  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 8  
    }  
  ]  
}
```