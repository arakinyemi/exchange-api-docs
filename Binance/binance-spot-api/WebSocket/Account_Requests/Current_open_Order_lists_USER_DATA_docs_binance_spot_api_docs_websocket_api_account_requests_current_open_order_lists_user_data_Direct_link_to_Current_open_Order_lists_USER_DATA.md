### Current open Order lists (USER\_DATA)​

```
{  
  "id": "3a4437e2-41a3-4c19-897c-9cadc5dce8b6",  
  "method": "openOrderLists.status",  
  "params": {  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "1bea8b157dd78c3da30359bddcd999e4049749fe50b828e620e12f64e8b433c9",  
    "timestamp": 1660801713831  
  }  
}
```

Query execution status of all open order lists.

If you need to continuously monitor order status updates, please consider using WebSocket Streams:

* [`userDataStream.start`](/docs/binance-spot-api-docs/websocket-api/user-data-stream-requests#user-data-stream-requests) request
* [`executionReport`](/docs/binance-spot-api-docs/user-data-stream#order-update) user data stream event

**Weight**:
6

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

**Data Source:**
Database

**Response:**

```
{  
  "id": "3a4437e2-41a3-4c19-897c-9cadc5dce8b6",  
  "status": 200,  
  "result": [  
    {  
      "orderListId": 0,  
      "contingencyType": "OCO",  
      "listStatusType": "EXEC_STARTED",  
      "listOrderStatus": "EXECUTING",  
      "listClientOrderId": "08985fedd9ea2cf6b28996",  
      "transactionTime": 1660801713793,  
      "symbol": "BTCUSDT",  
      "orders": [  
        {  
          "symbol": "BTCUSDT",  
          "orderId": 4,  
          "clientOrderId": "CUhLgTXnX5n2c0gWiLpV4d"  
        },  
        {  
          "symbol": "BTCUSDT",  
          "orderId": 5,  
          "clientOrderId": "1ZqG7bBuYwaF4SU8CwnwHm"  
        }  
      ]  
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