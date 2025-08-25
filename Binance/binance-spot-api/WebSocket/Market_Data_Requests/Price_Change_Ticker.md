### 24hr ticker price change statistics​

```
{  
  "id": "93fb61ef-89f8-4d6e-b022-4f035a3fadad",  
  "method": "ticker.24hr",  
  "params": {  
    "symbol": "BNBBTC"  
  }  
}
```

Get 24-hour rolling window price change statistics.

If you need to continuously monitor trading statistics, please consider using WebSocket Streams:

* [`<symbol>@ticker`](/docs/binance-spot-api-docs/web-socket-streams#individual-symbol-ticker-streams) or [`!ticker@arr`](/docs/binance-spot-api-docs/web-socket-streams#all-market-tickers-stream)
* [`<symbol>@miniTicker`](/docs/binance-spot-api-docs/web-socket-streams#individual-symbol-mini-ticker-stream) or [`!miniTicker@arr`](/docs/binance-spot-api-docs/web-socket-streams#all-market-mini-tickers-stream)

If you need different window sizes,
use the [`ticker`](/docs/binance-spot-api-docs/websocket-api/market-data-requests#rolling-window-price-change-statistics) request.

**Weight:**
Adjusted based on the number of requested symbols:

| Symbols | Weight |
| --- | --- |
| 1–20 | 2 |
| 21–100 | 40 |
| 101 or more | 80 |
| all symbols | 80 |

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | NO | Query ticker for a single symbol |
| `symbols` | ARRAY of STRING | Query ticker for multiple symbols |
| `type` | ENUM | NO | Ticker type: `FULL` (default) or `MINI` |

Notes:

* `symbol` and `symbols` cannot be used together.
* If no symbol is specified, returns information about all symbols currently trading on the exchange.

**Data Source:**
Memory

**Response:**

`FULL` type, for a single symbol:

```
{  
  "id": "93fb61ef-89f8-4d6e-b022-4f035a3fadad",  
  "status": 200,  
  "result": {  
    "symbol": "BNBBTC",  
    "priceChange": "0.00013900",  
    "priceChangePercent": "1.020",  
    "weightedAvgPrice": "0.01382453",  
    "prevClosePrice": "0.01362800",  
    "lastPrice": "0.01376700",  
    "lastQty": "1.78800000",  
    "bidPrice": "0.01376700",  
    "bidQty": "4.64600000",  
    "askPrice": "0.01376800",  
    "askQty": "14.31400000",  
    "openPrice": "0.01362800",  
    "highPrice": "0.01414900",  
    "lowPrice": "0.01346600",  
    "volume": "69412.40500000",  
    "quoteVolume": "959.59411487",  
    "openTime": 1660014164909,  
    "closeTime": 1660100564909,  
    "firstId": 194696115,       // First trade ID  
    "lastId": 194968287,        // Last trade ID  
    "count": 272173             // Number of trades  
  },  
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

`MINI` type, for a single symbol:

```
{  
  "id": "9fa2a91b-3fca-4ed7-a9ad-58e3b67483de",  
  "status": 200,  
  "result": {  
    "symbol": "BNBBTC",  
    "openPrice": "0.01362800",  
    "highPrice": "0.01414900",  
    "lowPrice": "0.01346600",  
    "lastPrice": "0.01376700",  
    "volume": "69412.40500000",  
    "quoteVolume": "959.59411487",  
    "openTime": 1660014164909,  
    "closeTime": 1660100564909,  
    "firstId": 194696115,       // First trade ID  
    "lastId": 194968287,        // Last trade ID  
    "count": 272173             // Number of trades  
  },  
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

If more than one symbol is requested, response returns an array:

```
{  
  "id": "901be0d9-fd3b-45e4-acd6-10c580d03430",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BNBBTC",  
      "priceChange": "0.00016500",  
      "priceChangePercent": "1.213",  
      "weightedAvgPrice": "0.01382508",  
      "prevClosePrice": "0.01360800",  
      "lastPrice": "0.01377200",  
      "lastQty": "1.01400000",  
      "bidPrice": "0.01377100",  
      "bidQty": "7.55700000",  
      "askPrice": "0.01377200",  
      "askQty": "4.37900000",  
      "openPrice": "0.01360700",  
      "highPrice": "0.01414900",  
      "lowPrice": "0.01346600",  
      "volume": "69376.27900000",  
      "quoteVolume": "959.13277091",  
      "openTime": 1660014615517,  
      "closeTime": 1660101015517,  
      "firstId": 194697254,  
      "lastId": 194969483,  
      "count": 272230  
    },  
    {  
      "symbol": "BTCUSDT",  
      "priceChange": "-938.06000000",  
      "priceChangePercent": "-3.938",  
      "weightedAvgPrice": "23265.34432003",  
      "prevClosePrice": "23819.17000000",  
      "lastPrice": "22880.91000000",  
      "lastQty": "0.00536000",  
      "bidPrice": "22880.40000000",  
      "bidQty": "0.00424000",  
      "askPrice": "22880.91000000",  
      "askQty": "0.04276000",  
      "openPrice": "23818.97000000",  
      "highPrice": "23933.25000000",  
      "lowPrice": "22664.69000000",  
      "volume": "153508.37606000",  
      "quoteVolume": "3571425225.04441220",  
      "openTime": 1660014615977,  
      "closeTime": 1660101015977,  
      "firstId": 1592019902,  
      "lastId": 1597301762,  
      "count": 5281861  
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