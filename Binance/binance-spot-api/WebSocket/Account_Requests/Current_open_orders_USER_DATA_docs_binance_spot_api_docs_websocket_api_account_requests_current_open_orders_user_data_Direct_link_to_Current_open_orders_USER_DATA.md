### Current open orders (USER\_DATA)​

```
{  
  "id": "55f07876-4f6f-4c47-87dc-43e5fff3f2e7",  
  "method": "openOrders.status",  
  "params": {  
    "symbol": "BTCUSDT",  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "d632b3fdb8a81dd44f82c7c901833309dd714fe508772a89b0a35b0ee0c48b89",  
    "timestamp": 1660813156812  
  }  
}
```

Query execution status of all open orders.

If you need to continuously monitor order status updates, please consider using WebSocket Streams:

* [`userDataStream.start`](/docs/binance-spot-api-docs/websocket-api/user-data-stream-requests#user-data-stream-requests) request
* [`executionReport`](/docs/binance-spot-api-docs/user-data-stream#order-update) user data stream event

**Weight:**
Adjusted based on the number of requested symbols:

| Parameter | Weight |
| --- | --- |
| `symbol` | 6 |
| none | 80 |

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | NO | If omitted, open orders for all symbols are returned |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

**Data Source:**
Memory => Database

**Response:**

Status reports for open orders are identical to [`order.status`](/docs/binance-spot-api-docs/websocket-api/account-requests#query-order-user_data).

Note that some fields are optional and included only for orders that set them.

Open orders are always returned as a flat list.
If all symbols are requested, use the `symbol` field to tell which symbol the orders belong to.

```
{  
  "id": "55f07876-4f6f-4c47-87dc-43e5fff3f2e7",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BTCUSDT",  
      "orderId": 12569099453,  
      "orderListId": -1,  
      "clientOrderId": "4d96324ff9d44481926157",  
      "price": "23416.10000000",  
      "origQty": "0.00847000",  
      "executedQty": "0.00720000",  
      "origQuoteOrderQty": "0.000000",  
      "cummulativeQuoteQty": "172.43931000",  
      "status": "PARTIALLY_FILLED",  
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
      "selfTradePreventionMode": "NONE"  
    }  
  ],  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 6  
    }  
  ]  
}
```

**Note:** The payload above does not show all fields that can appear. Please refer to Conditional fields in Order Responses.