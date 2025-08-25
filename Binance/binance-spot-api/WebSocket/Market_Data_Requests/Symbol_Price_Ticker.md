### Symbol price ticker​

```
{  
  "id": "043a7cf2-bde3-4888-9604-c8ac41fcba4d",  
  "method": "ticker.price",  
  "params": {  
    "symbol": "BNBBTC"  
  }  
}
```

Get the latest market price for a symbol.

If you need access to real-time price updates, please consider using WebSocket Streams:

* [`<symbol>@aggTrade`](/docs/binance-spot-api-docs/web-socket-streams#aggregate-trade-streams)
* [`<symbol>@trade`](/docs/binance-spot-api-docs/web-socket-streams#trade-streams)

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
| `symbol` | STRING | NO | Query price for a single symbol |
| `symbols` | ARRAY of STRING | Query price for multiple symbols |

Notes:

* `symbol` and `symbols` cannot be used together.
* If no symbol is specified, returns information about all symbols currently trading on the exchange.

**Data Source:**
Memory

**Response:**

```
{  
  "id": "043a7cf2-bde3-4888-9604-c8ac41fcba4d",  
  "status": 200,  
  "result": {  
    "symbol": "BNBBTC",  
    "price": "0.01361900"  
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
  "id": "e739e673-24c8-4adf-9cfa-b81f30330b09",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BNBBTC",  
      "price": "0.01363700"  
    },  
    {  
      "symbol": "BTCUSDT",  
      "price": "24267.15000000"  
    },  
    {  
      "symbol": "BNBBUSD",  
      "price": "331.10000000"  
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