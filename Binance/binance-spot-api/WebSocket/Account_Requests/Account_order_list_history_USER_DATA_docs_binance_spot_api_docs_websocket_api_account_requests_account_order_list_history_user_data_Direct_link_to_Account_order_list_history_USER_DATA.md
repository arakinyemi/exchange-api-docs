### Account order list history (USER\_DATA)​

```
{  
  "id": "8617b7b3-1b3d-4dec-94cd-eefd929b8ceb",  
  "method": "allOrderLists",  
  "params": {  
    "startTime": 1660780800000,  
    "endTime": 1660867200000,  
    "limit": 5,  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "c8e1484db4a4a02d0e84dfa627eb9b8298f07ebf12fcc4eaf86e4a565b2712c2",  
    "timestamp": 1661955123341  
  }  
}
```

Query information about all your order lists, filtered by time range.

**Weight:**
20

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `fromId` | INT | NO | Order list ID to begin at |
| `startTime` | LONG | NO |  |
| `endTime` | LONG | NO |  |
| `limit` | INT | NO | Default: 500; Maximum: 1000 |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

Notes:

* If `startTime` and/or `endTime` are specified, `fromId` is ignored.

  Order lists are filtered by `transactionTime` of the last order list execution status update.
* If `fromId` is specified, return order lists with order list ID >= `fromId`.
* If no condition is specified, the most recent order lists are returned.
* The time between `startTime` and `endTime` can't be longer than 24 hours.

**Data Source:**
Database

**Response:**

Status reports for order lists are identical to [`orderList.status`](/docs/binance-spot-api-docs/websocket-api/account-requests#query-order-list-user_data).

```
{  
  "id": "8617b7b3-1b3d-4dec-94cd-eefd929b8ceb",  
  "status": 200,  
  "result": [  
    {  
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