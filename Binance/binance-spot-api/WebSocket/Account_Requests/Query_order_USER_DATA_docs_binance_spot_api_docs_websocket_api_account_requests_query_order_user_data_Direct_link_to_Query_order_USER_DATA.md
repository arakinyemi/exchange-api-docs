### Query order (USER\_DATA)​

```
{  
  "id": "aa62318a-5a97-4f3b-bdc7-640bbe33b291",  
  "method": "order.status",  
  "params": {  
    "symbol": "BTCUSDT",  
    "orderId": 12569099453,  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "2c3aab5a078ee4ea465ecd95523b77289f61476c2f238ec10c55ea6cb11a6f35",  
    "timestamp": 1660801720951  
  }  
}
```

Check execution status of an order.

**Weight:**
4

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `orderId` | LONG | YES | Lookup order by `orderId` |
| `origClientOrderId` | STRING | Lookup order by `clientOrderId` |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than 60000 |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

Notes:

* If both `orderId` and `origClientOrderId` are provided, the `orderId` is searched first, then the `origClientOrderId` from that result is checked against that order. If both conditions are not met the request will be rejected.
* For some historical orders the `cummulativeQuoteQty` response field may be negative,
  meaning the data is not available at this time.

**Data Source:**
Memory => Database

**Response:**

```
{  
  "id": "aa62318a-5a97-4f3b-bdc7-640bbe33b291",  
  "status": 200,  
  "result": {  
    "symbol": "BTCUSDT",  
    "orderId": 12569099453,  
    "orderListId": -1,                  // set only for orders of an order list  
    "clientOrderId": "4d96324ff9d44481926157",  
    "price": "23416.10000000",  
    "origQty": "0.00847000",  
    "executedQty": "0.00847000",  
    "cummulativeQuoteQty": "198.33521500",  
    "status": "FILLED",  
    "timeInForce": "GTC",  
    "type": "LIMIT",  
    "side": "SELL",  
    "stopPrice": "0.00000000",          // always present, zero if order type does not use stopPrice  
    "trailingDelta": 10,                // present only if trailingDelta set for the order  
    "trailingTime": -1,                 // present only if trailingDelta set for the order  
    "icebergQty": "0.00000000",         // always present, zero for non-iceberg orders  
    "time": 1660801715639,              // time when the order was placed  
    "updateTime": 1660801717945,        // time of the last update to the order  
    "isWorking": true,  
    "workingTime": 1660801715639,  
    "origQuoteOrderQty": "0.00000000",   // always present, zero if order type does not use quoteOrderQty  
    "strategyId": 37463720,             // present only if strategyId set for the order  
    "strategyType": 1000000,            // present only if strategyType set for the order  
    "selfTradePreventionMode": "NONE",  
    "preventedMatchId": 0,              // present only if the order expired due to STP  
    "preventedQuantity": "1.200000"     // present only if the order expired due to STP  
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

**Note:** The payload above does not show all fields that can appear. Please refer to Conditional fields in Order Responses.