### Symbol order book ticker​

```
{  
  "id": "057deb3a-2990-41d1-b58b-98ea0f09e1b4",  
  "method": "ticker.book",  
  "params": {  
    "symbols": [  
      "BNBBTC",  
      "BTCUSDT"  
    ]  
  }  
}
```

Get the current best price and quantity on the order book.

If you need access to real-time order book ticker updates, please consider using WebSocket Streams:

* [`<symbol>@bookTicker`](/docs/binance-spot-api-docs/web-socket-streams#individual-symbol-book-ticker-streams)

**Weight:**
Adjusted based on the number of requested symbols:

| Parameter | Weight |
| --- | --- |
| `symbol` | 2 |
| `symbols` | 4 |
| none | 4 |

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | NO | Query ticker for a single symbol |
| `symbols` | ARRAY of STRING | Query ticker for multiple symbols |

Notes:

* `symbol` and `symbols` cannot be used together.
* If no symbol is specified, returns information about all symbols currently trading on the exchange.

**Data Source:**
Memory

**Response:**

```
{  
  "id": "9d32157c-a556-4d27-9866-66760a174b57",  
  "status": 200,  
  "result": {  
    "symbol": "BNBBTC",  
    "bidPrice": "0.01358000",  
    "bidQty": "12.53400000",  
    "askPrice": "0.01358100",  
    "askQty": "17.83700000"  
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
  "id": "057deb3a-2990-41d1-b58b-98ea0f09e1b4",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BNBBTC",  
      "bidPrice": "0.01358000",  
      "bidQty": "12.53400000",  
      "askPrice": "0.01358100",  
      "askQty": "17.83700000"  
    },  
    {  
      "symbol": "BTCUSDT",  
      "bidPrice": "23980.49000000",  
      "bidQty": "0.01000000",  
      "askPrice": "23981.31000000",  
      "askQty": "0.01512000"  
    }  
  ],  
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

* Order book
* Recent trades
* Historical trades
* Aggregate trades
* Klines
* UI Klines
* Current average price
* 24hr ticker price change statistics
* Trading Day Ticker
* Rolling window price change statistics
* Symbol price ticker
* Symbol order book ticker