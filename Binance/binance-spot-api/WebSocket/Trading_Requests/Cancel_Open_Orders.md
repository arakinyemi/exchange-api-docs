### Cancel open orders 

```json
{  
  "id": "778f938f-9041-4b88-9914-efbf64eeacc8",  
  "method": "openOrders.cancelAll",  
  "params": {  
    "symbol": "BTCUSDT",  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "773f01b6e3c2c9e0c1d217bc043ce383c1ddd6f0e25f8d6070f2b66a6ceaf3a5",  
    "timestamp": 1660805557200  
  }  
}
```

Cancel all open orders on a symbol.
This includes orders that are part of an order list.

**Weight:**
1

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

**Data Source:**
Matching Engine

**Response:**

Cancellation reports for orders and order lists have the same format as in [`order.cancel`](/docs/binance-spot-api-docs/websocket-api/trading-requests#cancel-order-trade).

```json
{  
  "id": "778f938f-9041-4b88-9914-efbf64eeacc8",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BTCUSDT",  
      "origClientOrderId": "4d96324ff9d44481926157",  
      "orderId": 12569099453,  
      "orderListId": -1,  
      "clientOrderId": "91fe37ce9e69c90d6358c0",  
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
      "stopPrice": "0.00000000",  
      "trailingDelta": 0,  
      "trailingTime": -1,  
      "icebergQty": "0.00000000",  
      "strategyId": 37463720,  
      "strategyType": 1000000,  
      "selfTradePreventionMode": "NONE"  
    },  
    {  
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
    }  
  ],  
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