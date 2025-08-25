### Historical trades​

```
{  
  "id": "cffc9c7d-4efc-4ce0-b587-6b87448f052a",  
  "method": "trades.historical",  
  "params": {  
    "symbol": "BNBBTC",  
    "fromId": 0,  
    "limit": 1  
  }  
}
```

Get historical trades.

**Weight:**
25

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `fromId` | INT | NO | Trade ID to begin at |
| `limit` | INT | NO | Default: 500; Maximum: 1000 |

Notes:

* If `fromId` is not specified, the most recent trades are returned.

**Data Source:**
Database

**Response:**

```
{  
  "id": "cffc9c7d-4efc-4ce0-b587-6b87448f052a",  
  "status": 200,  
  "result": [  
    {  
      "id": 0,  
      "price": "0.00005000",  
      "qty": "40.00000000",  
      "quoteQty": "0.00200000",  
      "time": 1500004800376,  
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
      "count": 10  
    }  
  ]  
}
```