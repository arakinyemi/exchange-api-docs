### Account trade history (USER\_DATA)​

```
{  
  "id": "f4ce6a53-a29d-4f70-823b-4ab59391d6e8",  
  "method": "myTrades",  
  "params": {  
    "symbol": "BTCUSDT",  
    "startTime": 1660780800000,  
    "endTime": 1660867200000,  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "c5a5ffb79fd4f2e10a92f895d488943a57954edf5933bde3338dfb6ea6d6eefc",  
    "timestamp": 1661955125250  
  }  
}
```

Query information about all your trades, filtered by time range.

**Weight:**

| Condition | Weight |
| --- | --- |
| Without orderId | 20 |
| With orderId | 5 |

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `orderId` | LONG | NO |  |
| `startTime` | LONG | NO |  |
| `endTime` | LONG | NO |  |
| `fromId` | INT | NO | First trade ID to query |
| `limit` | INT | NO | Default: 500; Maximum: 1000 |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

Notes:

* If `fromId` is specified, return trades with trade ID >= `fromId`.
* If `startTime` and/or `endTime` are specified, trades are filtered by execution time (`time`).

  `fromId` cannot be used together with `startTime` and `endTime`.
* If `orderId` is specified, only trades related to that order are returned.

  `startTime` and `endTime` cannot be used together with `orderId`.
* If no condition is specified, the most recent trades are returned.
* The time between `startTime` and `endTime` can't be longer than 24 hours.

**Data Source:**
Memory => Database

**Response:**

```
{  
  "id": "f4ce6a53-a29d-4f70-823b-4ab59391d6e8",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BTCUSDT",  
      "id": 1650422481,  
      "orderId": 12569099453,  
      "orderListId": -1,  
      "price": "23416.10000000",  
      "qty": "0.00635000",  
      "quoteQty": "148.69223500",  
      "commission": "0.00000000",  
      "commissionAsset": "BNB",  
      "time": 1660801715793,  
      "isBuyer": false,  
      "isMaker": true,  
      "isBestMatch": true  
    },  
    {  
      "symbol": "BTCUSDT",  
      "id": 1650422482,  
      "orderId": 12569099453,  
      "orderListId": -1,  
      "price": "23416.50000000",  
      "qty": "0.00212000",  
      "quoteQty": "49.64298000",  
      "commission": "0.00000000",  
      "commissionAsset": "BNB",  
      "time": 1660801715793,  
      "isBuyer": false,  
      "isMaker": true,  
      "isBestMatch": true  
    }  
  ],  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 20  
    }  
  ]  
}
```