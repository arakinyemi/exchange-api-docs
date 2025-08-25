### Account order history (USER\_DATA)​

```
{  
  "id": "734235c2-13d2-4574-be68-723e818c08f3",  
  "method": "allOrders",  
  "params": {  
    "symbol": "BTCUSDT",  
    "startTime": 1660780800000,  
    "endTime": 1660867200000,  
    "limit": 5,  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "f50a972ba7fad92842187643f6b930802d4e20bce1ba1e788e856e811577bd42",  
    "timestamp": 1661955123341  
  }  
}
```

Query information about all your orders – active, canceled, filled – filtered by time range.

**Weight:**
20

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `orderId` | LONG | NO | Order ID to begin at |
| `startTime` | LONG | NO |  |
| `endTime` | LONG | NO |  |
| `limit` | INT | NO | Default: 500; Maximum: 1000 |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

Notes:

* If `startTime` and/or `endTime` are specified, `orderId` is ignored.

  Orders are filtered by `time` of the last execution status update.
* If `orderId` is specified, return orders with order ID >= `orderId`.
* If no condition is specified, the most recent orders are returned.
* For some historical orders the `cummulativeQuoteQty` response field may be negative,
  meaning the data is not available at this time.
* The time between `startTime` and `endTime` can't be longer than 24 hours.

**Data Source:**
Database

**Response:**

Status reports for orders are identical to [`order.status`](/docs/binance-spot-api-docs/websocket-api/account-requests#query-order-user_data).

Note that some fields are optional and included only for orders that set them.

```
{  
  "id": "734235c2-13d2-4574-be68-723e818c08f3",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BTCUSDT",  
      "orderId": 12569099453,  
      "orderListId": -1,  
      "clientOrderId": "4d96324ff9d44481926157",  
      "price": "23416.10000000",  
      "origQty": "0.00847000",  
      "executedQty": "0.00847000",  
      "cummulativeQuoteQty": "198.33521500",  
      "status": "FILLED",  
      "timeInForce": "GTC",  
      "type": "LIMIT",  
      "side": "SELL",  
      "stopPrice": "0.00000000",  
      "icebergQty": "0.00000000",  
      "time": 1660801715639,  
      "updateTime": 1660801717945,  
      "isWorking": true,  
      "workingTime": 1660801715639,  
      "origQuoteOrderQty": "0.00000000",  
      "selfTradePreventionMode": "NONE",  
      "preventedMatchId": 0,            // This field only appears if the order expired due to STP.  
      "preventedQuantity": "1.200000"   // This field only appears if the order expired due to STP.  
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