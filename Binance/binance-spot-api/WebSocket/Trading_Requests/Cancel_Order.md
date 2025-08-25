### Cancel order 

```
{  
  "id": "5633b6a2-90a9-4192-83e7-925c90b6a2fd",  
  "method": "order.cancel",  
  "params": {  
    "symbol": "BTCUSDT",  
    "origClientOrderId": "4d96324ff9d44481926157",  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "33d5b721f278ae17a52f004a82a6f68a70c68e7dd6776ed0be77a455ab855282",  
    "timestamp": 1660801715830  
  }  
}
```

Cancel an active order.

**Weight:**
1

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `orderId` | LONG | YES | Cancel order by `orderId` |
| `origClientOrderId` | STRING | Cancel order by `clientOrderId` |
| `newClientOrderId` | STRING | NO | New ID for the canceled order. Automatically generated if not sent |
| `cancelRestrictions` | ENUM | NO | Supported values:  `ONLY_NEW` - Cancel will succeed if the order status is `NEW`.  `ONLY_PARTIALLY_FILLED` - Cancel will succeed if order status is `PARTIALLY_FILLED`. |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than 60000 |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

Notes:

* If both `orderId` and `origClientOrderId` parameters are provided, the `orderId` is searched first, then the `origClientOrderId` from that result is checked against that order. If both conditions are not met the request will be rejected.
* `newClientOrderId` will replace `clientOrderId` of the canceled order, freeing it up for new orders.
* If you cancel an order that is a part of an order list, the entire order list is canceled.
* The performance for canceling an order (single cancel or as part of a cancel-replace) is always better when only `orderId` is sent. Sending `origClientOrderId` or both `orderId` + `origClientOrderId` will be slower.

**Data Source:**
Matching Engine

**Response:**

When an individual order is canceled:

```
{  
  "id": "5633b6a2-90a9-4192-83e7-925c90b6a2fd",  
  "status": 200,  
  "result": {  
    "symbol": "BTCUSDT",  
    "origClientOrderId": "4d96324ff9d44481926157",  // clientOrderId that was canceled  
    "orderId": 12569099453,  
    "orderListId": -1,                              // set only for legs of an order list  
    "clientOrderId": "91fe37ce9e69c90d6358c0",      // newClientOrderId from request  
    "transactTime": 1684804350068,  
    "price": "23416.10000000",  
    "origQty": "0.00847000",  
    "executedQty": "0.00001000",  
    "origQuoteOrderQty": "0.000000",  
    "cummulativeQuoteQty": "0.23416100",  
    "status": "CANCELED",  
    "timeInForce": "GTC",  
    "type": "LIMIT",  
    "side": "SELL",  
    "stopPrice": "0.00000000",          // present only if stopPrice set for the order  
    "trailingDelta": 0,                 // present only if trailingDelta set for the order  
    "icebergQty": "0.00000000",         // present only if icebergQty set for the order  
    "strategyId": 37463720,             // present only if strategyId set for the order  
    "strategyType": 1000000,            // present only if strategyType set for the order  
    "selfTradePreventionMode": "NONE"  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 1  
    }  
  ]  
}
```

When an order list is canceled:

```
{  
  "id": "16eaf097-bbec-44b9-96ff-e97e6e875870",  
  "status": 200,  
  "result": {  
    "orderListId": 19431,  
    "contingencyType": "OCO",  
    "listStatusType": "ALL_DONE",  
    "listOrderStatus": "ALL_DONE",  
    "listClientOrderId": "iuVNVJYYrByz6C4yGOPPK0",  
    "transactionTime": 1660803702431,  
    "symbol": "BTCUSDT",  
    "orders": [  
      {  
        "symbol": "BTCUSDT",  
        "orderId": 12569099453,  
        "clientOrderId": "bX5wROblo6YeDwa9iTLeyY"  
      },  
      {  
        "symbol": "BTCUSDT",  
        "orderId": 12569099454,  
        "clientOrderId": "Tnu2IP0J5Y4mxw3IATBfmW"  
      }  
    ],  
    //order list order's status format is the same as for individual orders.  
    "orderReports": [  
      {  
        "symbol": "BTCUSDT",  
        "origClientOrderId": "bX5wROblo6YeDwa9iTLeyY",  
        "orderId": 12569099453,  
        "orderListId": 19431,  
        "clientOrderId": "OFFXQtxVFZ6Nbcg4PgE2DA",  
        "transactTime": 1684804350068,  
        "price": "23450.50000000",  
        "origQty": "0.00850000",  
        "executedQty": "0.00000000",  
        "origQuoteOrderQty": "0.000000",  
        "cummulativeQuoteQty": "0.00000000",  
        "status": "CANCELED",  
        "timeInForce": "GTC",  
        "type": "STOP_LOSS_LIMIT",  
        "side": "BUY",  
        "stopPrice": "23430.00000000",  
        "selfTradePreventionMode": "NONE"  
      },  
      {  
        "symbol": "BTCUSDT",  
        "origClientOrderId": "Tnu2IP0J5Y4mxw3IATBfmW",  
        "orderId": 12569099454,  
        "orderListId": 19431,  
        "clientOrderId": "OFFXQtxVFZ6Nbcg4PgE2DA",  
        "transactTime": 1684804350068,  
        "price": "23400.00000000",  
        "origQty": "0.00850000",  
        "executedQty": "0.00000000",  
        "origQuoteOrderQty": "0.000000",  
        "cummulativeQuoteQty": "0.00000000",  
        "status": "CANCELED",  
        "timeInForce": "GTC",  
        "type": "LIMIT_MAKER",  
        "side": "BUY",  
        "selfTradePreventionMode": "NONE"  
      }  
    ]  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 1  
    }  
  ]  
}
```

**Note:** The payload above does not show all fields that can appear. Please refer to Conditional fields in Order Responses.

**Regarding `cancelRestrictions`**

* If the `cancelRestrictions` value is not any of the supported values, the error will be:

```
{  
    "code": -1145,  
    "msg": "Invalid cancelRestrictions"  
}
```

* If the order did not pass the conditions for `cancelRestrictions`, the error will be:

```
{  
    "code": -2011,  
    "msg": "Order was not canceled due to cancel restrictions."  
}
```