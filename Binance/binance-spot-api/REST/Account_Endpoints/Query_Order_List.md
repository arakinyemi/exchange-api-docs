### Query Order list 
```
GET /api/v3/orderList
```

Retrieves a specific order list based on provided optional parameters.

**Weight:**
4

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| orderListId | LONG | NO\* | Query order list by `orderListId`.  `orderListId` or `origClientOrderId` must be provided. |
| origClientOrderId | STRING | NO\* | Query order list by `listClientOrderId`.  `orderListId` or `origClientOrderId` must be provided. |
| recvWindow | LONG | NO | The value cannot be greater than `60000` |
| timestamp | LONG | YES |  |

**Data Source:**
Database

**Response:**

```
{  
  "orderListId": 27,  
  "contingencyType": "OCO",  
  "listStatusType": "EXEC_STARTED",  
  "listOrderStatus": "EXECUTING",  
  "listClientOrderId": "h2USkA5YQpaXHPIrkd96xE",  
  "transactionTime": 1565245656253,  
  "symbol": "LTCBTC",  
  "orders": [  
    {  
      "symbol": "LTCBTC",  
      "orderId": 4,  
      "clientOrderId": "qD1gy3kc3Gx0rihm9Y3xwS"  
    },  
    {  
      "symbol": "LTCBTC",  
      "orderId": 5,  
      "clientOrderId": "ARzZ9I00CPM8i3NhmU9Ega"  
    }  
  ]  
}
```