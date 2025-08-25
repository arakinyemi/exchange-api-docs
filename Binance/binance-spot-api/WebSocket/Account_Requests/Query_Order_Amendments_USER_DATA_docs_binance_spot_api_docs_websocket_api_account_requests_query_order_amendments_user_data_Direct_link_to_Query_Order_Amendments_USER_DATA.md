### Query Order Amendments (USER\_DATA)​

```
{  
  "id": "6f5ebe91-01d9-43ac-be99-57cf062e0e30",  
  "method": "order.amendments",  
  "params": {  
  "orderId": "23",  
  "recvWindow": 5000,  
  "symbol": "BTCUSDT",  
  "timestamp": 1741925524887,  
  "apiKey": "N3Swv7WaBF7S2rzA12UkPunM3udJiDddbgv1W7CzFGnsQXH9H62zzSCST0CndjeE",  
  "signature": "0eed2e9d95b6868ea5ec21da0d14538192ef344c30ecf9fe83d58631699334dc"  
  }  
}
```

Queries all amendments of a single order.

**Weight**:
4

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| symbol | STRING | YES |  |
| orderId | LONG | YES |  |
| fromExecutionId | LONG | NO |  |
| limit | INT | NO | Default:500; Maximum: 1000 |
| recvWindow | LONG | NO | The value cannot be greater than `60000`. |
| timestamp | LONG | YES |  |

**Data Source:**
Database

**Response:**

```
{  
  "id": "6f5ebe91-01d9-43ac-be99-57cf062e0e30",  
  "status": 200,  
  "result":  
  [  
    {  
      "symbol": "BTCUSDT",  
      "orderId": 23,  
      "executionId": 60,  
      "origClientOrderId": "my_pending_order",  
      "newClientOrderId": "xbxXh5SSwaHS7oUEOCI88B",  
      "origQty": "7.00000000",  
      "newQty": "5.00000000",  
      "time": 1741924229819  
    }  
  ],  
  "rateLimits":  
  [  
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

* Account information (USER\_DATA)
* Query order (USER\_DATA)
* Current open orders (USER\_DATA)
* Account order history (USER\_DATA)
* Query Order list (USER\_DATA)
* Current open Order lists (USER\_DATA)
* Account order list history (USER\_DATA)
* Account trade history (USER\_DATA)
* Unfilled Order Count (USER\_DATA)
* Account prevented matches (USER\_DATA)
* Account allocations (USER\_DATA)
* Account Commission Rates (USER\_DATA)
* Query Order Amendments (USER\_DATA)