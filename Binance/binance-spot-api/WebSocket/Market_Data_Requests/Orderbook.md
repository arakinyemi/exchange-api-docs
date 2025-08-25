### Order book​

```
{  
  "id": "51e2affb-0aba-4821-ba75-f2625006eb43",  
  "method": "depth",  
  "params": {  
    "symbol": "BNBBTC",  
    "limit": 5  
  }  
}
```

Get current order book.

Note that this request returns limited market depth.

If you need to continuously monitor order book updates, please consider using WebSocket Streams:

* [`<symbol>@depth<levels>`](/docs/binance-spot-api-docs/web-socket-streams#partial-book-depth-streams)
* [`<symbol>@depth`](/docs/binance-spot-api-docs/web-socket-streams#diff-depth-stream)

You can use `depth` request together with `<symbol>@depth` streams to maintain a local order book.

**Weight:**
Adjusted based on the limit:

| Limit | Weight |
| --- | --- |
| 1–100 | 5 |
| 101–500 | 25 |
| 501–1000 | 50 |
| 1001–5000 | 250 |

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `limit` | INT | NO | Default: 100; Maximum: 5000 |

**Data Source:**
Memory

**Response:**

```
{  
  "id": "51e2affb-0aba-4821-ba75-f2625006eb43",  
  "status": 200,  
  "result": {  
    "lastUpdateId": 2731179239,  
    // Bid levels are sorted from highest to lowest price.  
    "bids": [  
      [  
        "0.01379900",   // Price  
        "3.43200000"    // Quantity  
      ],  
      [  
        "0.01379800",  
        "3.24300000"  
      ],  
      [  
        "0.01379700",  
        "10.45500000"  
      ],  
      [  
        "0.01379600",  
        "3.82100000"  
      ],  
      [  
        "0.01379500",  
        "10.26200000"  
      ]  
    ],  
    // Ask levels are sorted from lowest to highest price.  
    "asks": [  
      [  
        "0.01380000",  
        "5.91700000"  
      ],  
      [  
        "0.01380100",  
        "6.01400000"  
      ],  
      [  
        "0.01380200",  
        "0.26800000"  
      ],  
      [  
        "0.01380300",  
        "0.33800000"  
      ],  
      [  
        "0.01380400",  
        "0.26800000"  
      ]  
    ]  
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