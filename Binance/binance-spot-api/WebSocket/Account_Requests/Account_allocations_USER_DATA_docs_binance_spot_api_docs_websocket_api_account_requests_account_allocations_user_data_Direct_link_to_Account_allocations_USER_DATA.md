### Account allocations (USER\_DATA)​

```
{  
  "id": "g4ce6a53-a39d-4f71-823b-4ab5r391d6y8",  
  "method": "myAllocations",  
  "params": {  
    "symbol": "BTCUSDT",  
    "orderId": 500,  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "c5a5ffb79fd4f2e10a92f895d488943a57954edf5933bde3338dfb6ea6d6eefc",  
    "timestamp": 1673923281052  
  }  
}
```

Retrieves allocations resulting from SOR order placement.

**Weight:**
20

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | Yes |  |
| `startTime` | LONG | No |  |
| `endTime` | LONG | No |  |
| `fromAllocationId` | INT | No |  |
| `limit` | INT | No | Default: 500; Maximum: 1000 |
| `orderId` | LONG | No |  |
| `recvWindow` | LONG | No | The value cannot be greater than `60000` |
| `timestamp` | LONG | No |  |

Supported parameter combinations:

| Parameters | Response |
| --- | --- |
| `symbol` | allocations from oldest to newest |
| `symbol` + `startTime` | oldest allocations since `startTime` |
| `symbol` + `endTime` | newest allocations until `endTime` |
| `symbol` + `startTime` + `endTime` | allocations within the time range |
| `symbol` + `fromAllocationId` | allocations by allocation ID |
| `symbol` + `orderId` | allocations related to an order starting with oldest |
| `symbol` + `orderId` + `fromAllocationId` | allocations related to an order by allocation ID |

**Note:** The time between `startTime` and `endTime` can't be longer than 24 hours.

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
      "allocationId": 0,  
      "allocationType": "SOR",  
      "orderId": 500,  
      "orderListId": -1,  
      "price": "1.00000000",  
      "qty": "0.10000000",  
      "quoteQty": "0.10000000",  
      "commission": "0.00000000",  
      "commissionAsset": "BTC",  
      "time": 1687319487614,  
      "isBuyer": false,  
      "isMaker": false,  
      "isAllocator": false  
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