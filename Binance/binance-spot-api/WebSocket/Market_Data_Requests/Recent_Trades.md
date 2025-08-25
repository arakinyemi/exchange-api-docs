### Recent trades​

```
{  
  "id": "409a20bd-253d-41db-a6dd-687862a5882f",  
  "method": "trades.recent",  
  "params": {  
    "symbol": "BNBBTC",  
    "limit": 1  
  }  
}
```

Get recent trades.

If you need access to real-time trading activity, please consider using WebSocket Streams:

* [`<symbol>@trade`](/docs/binance-spot-api-docs/web-socket-streams#trade-streams)

**Weight:**
25

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `limit` | INT | NO | Default: 500; Maximum: 1000 |

**Data Source:**
Memory

**Response:**

```
{  
  "id": "409a20bd-253d-41db-a6dd-687862a5882f",  
  "status": 200,  
  "result": [  
    {  
      "id": 194686783,  
      "price": "0.01361000",  
      "qty": "0.01400000",  
      "quoteQty": "0.00019054",  
      "time": 1660009530807,  
      "isBuyerMaker": true,  
      "isBestMatch": true  
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