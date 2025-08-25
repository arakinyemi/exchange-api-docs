### Query order 

```
GET /api/v3/order
```

Check an order's status.

**Weight:**
4

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| symbol | STRING | YES |  |
| orderId | LONG | NO |  |
| origClientOrderId | STRING | NO |  |
| recvWindow | LONG | NO | The value cannot be greater than `60000` |
| timestamp | LONG | YES |  |

**Notes:**

* Either `orderId` or `origClientOrderId` must be sent.
* If both `orderId` and `origClientOrderId` are provided, the `orderId` is searched first, then the `origClientOrderId` from that result is checked against that order. If both conditions are not met the request will be rejected.
* For some historical orders `cummulativeQuoteQty` will be < 0, meaning the data is not available at this time.

**Data Source:**
Memory => Database

**Response:**

```json
{  
  "symbol": "LTCBTC",  
  "orderId": 1,  
  "orderListId": -1,                 // This field will always have a value of -1 if not an order list.  
  "clientOrderId": "myOrder1",  
  "price": "0.1",  
  "origQty": "1.0",  
  "executedQty": "0.0",  
  "cummulativeQuoteQty": "0.0",  
  "status": "NEW",  
  "timeInForce": "GTC",  
  "type": "LIMIT",  
  "side": "BUY",  
  "stopPrice": "0.0",  
  "icebergQty": "0.0",  
  "time": 1499827319559,  
  "updateTime": 1499827319559,  
  "isWorking": true,  
  "workingTime":1499827319559,  
  "origQuoteOrderQty": "0.000000",  
  "selfTradePreventionMode": "NONE"  
}
```

**Note:** The payload above does not show all fields that can appear. Please refer to Conditional fields in Trading Endpoints.