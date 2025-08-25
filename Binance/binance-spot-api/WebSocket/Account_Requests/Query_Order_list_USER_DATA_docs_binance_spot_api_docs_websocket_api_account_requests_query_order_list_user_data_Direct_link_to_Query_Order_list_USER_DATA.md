### Query Order list (USER\_DATA)​

```
{  
  "id": "b53fd5ff-82c7-4a04-bd64-5f9dc42c2100",  
  "method": "orderList.status",  
  "params": {  
    "origClientOrderId": "08985fedd9ea2cf6b28996",  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "d12f4e8892d46c0ddfbd43d556ff6d818581b3be22a02810c2c20cb719aed6a4",  
    "timestamp": 1660801713965  
  }  
}
```

Check execution status of an Order list.

For execution status of individual orders, use [`order.status`](/docs/binance-spot-api-docs/websocket-api/account-requests#query-order-user_data).

**Weight:**
4

**Parameters**:

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `origClientOrderId` | STRING | NO\* | Query order list by `listClientOrderId`. `orderListId` or `origClientOrderId` must be provided. |
| `orderListId` | INT | Query order list by `orderListId`. `orderListId` or `origClientOrderId` must be provided. |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than 60000 |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

Notes:

* `origClientOrderId` refers to `listClientOrderId` of the order list itself.
* If both `origClientOrderId` and `orderListId` parameters are specified,
  only `origClientOrderId` is used and `orderListId` is ignored.

**Data Source:**
Database

**Response:**

```
{  
  "id": "b53fd5ff-82c7-4a04-bd64-5f9dc42c2100",  
  "status": 200,  
  "result": {  
    "orderListId": 1274512,  
    "contingencyType": "OCO",  
    "listStatusType": "EXEC_STARTED",  
    "listOrderStatus": "EXECUTING",  
    "listClientOrderId": "08985fedd9ea2cf6b28996",  
    "transactionTime": 1660801713793,  
    "symbol": "BTCUSDT",  
    "orders": [  
      {  
        "symbol": "BTCUSDT",  
        "orderId": 12569138901,  
        "clientOrderId": "BqtFCj5odMoWtSqGk2X9tU"  
      },  
      {  
        "symbol": "BTCUSDT",  
        "orderId": 12569138902,  
        "clientOrderId": "jLnZpj5enfMXTuhKB1d0us"  
      }  
    ]  
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