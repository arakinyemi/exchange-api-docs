### Account prevented matches (USER\_DATA)​

```
{  
  "id": "g4ce6a53-a39d-4f71-823b-4ab5r391d6y8",  
  "method": "myPreventedMatches",  
  "params": {  
    "symbol": "BTCUSDT",  
    "orderId": 35,  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "c5a5ffb79fd4f2e10a92f895d488943a57954edf5933bde3338dfb6ea6d6eefc",  
    "timestamp": 1673923281052  
  }  
}
```

Displays the list of orders that were expired due to STP.

These are the combinations supported:

* `symbol` + `preventedMatchId`
* `symbol` + `orderId`
* `symbol` + `orderId` + `fromPreventedMatchId` (`limit` will default to 500)
* `symbol` + `orderId` + `fromPreventedMatchId` + `limit`

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| symbol | STRING | YES |  |
| preventedMatchId | LONG | NO |  |
| orderId | LONG | NO |  |
| fromPreventedMatchId | LONG | NO |  |
| limit | INT | NO | Default: `500`; Maximum: `1000` |
| recvWindow | LONG | NO | The value cannot be greater than `60000` |
| timestamp | LONG | YES |  |

**Weight**

| Case | Weight |
| --- | --- |
| If `symbol` is invalid | 2 |
| Querying by `preventedMatchId` | 2 |
| Querying by `orderId` | 20 |

**Data Source:**

Database

**Response:**

```
{  
  "id": "g4ce6a53-a39d-4f71-823b-4ab5r391d6y8",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BTCUSDT",  
      "preventedMatchId": 1,  
      "takerOrderId": 5,  
      "makerSymbol": "BTCUSDT",  
      "makerOrderId": 3,  
      "tradeGroupId": 1,  
      "selfTradePreventionMode": "EXPIRE_MAKER",  
      "price": "1.100000",  
      "makerPreventedQuantity": "1.300000",  
      "transactTime": 1669101687094  
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