### Order Amend Keep Priority (TRADE)​

```
{  
  "id": "56374a46-3061-486b-a311-89ee972eb648",  
  "method": "order.amend.keepPriority",  
  "params": {  
  "newQty": "5",  
  "origClientOrderId": "my_test_order1",  
  "recvWindow": 5000,  
  "symbol": "BTCUSDT",  
  "timestamp": 1741922620419,  
  "apiKey": "Rl1KOMDCpSg6xviMYOkNk9ENUB5QOTnufXukVe0Ijd40yduAlpHn78at3rJyJN4F",  
  "signature": "fa49c0c4ebc331c6ebd3fcb20deb387f60081ea858eebe6e35aa6fcdf2a82e08"  
  }  
}
```

Reduce the quantity of an existing open order.

This adds 0 orders to the `EXCHANGE_MAX_ORDERS` filter and the `MAX_NUM_ORDERS` filter.

Read Order Amend Keep Priority FAQ to learn more.

**Weight**: 4

**Unfilled Order Count:**
0

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| symbol | STRING | YES |  |
| orderId | LONG | NO\* | `orderId` or `origClientOrderId` must be sent |
| origClientOrderId | STRING | NO\* | `orderId` or `origClientOrderId` must be sent |
| newClientOrderId | STRING | NO\* | The new client order ID for the order after being amended.   If not sent, one will be randomly generated.   It is possible to reuse the current clientOrderId by sending it as the `newClientOrderId`. |
| newQty | DECIMAL | YES | `newQty` must be greater than 0 and less than the order's quantity. |
| recvWindow | LONG | NO | The value cannot be greater than `60000`. |
| timestamp | LONG | YES |  |

**Data Source**: Matching Engine

**Response:**

Response for a single order:

```
{  
  "id": "56374a46-3061-486b-a311-89ee972eb648",  
  "status": 200,  
  "result":  
  {  
    "transactTime": 1741923284382,  
    "executionId": 16,  
    "amendedOrder":  
    {  
      "symbol": "BTCUSDT",  
      "orderId": 12,  
      "orderListId": -1,  
      "origClientOrderId": "my_test_order1",  
      "clientOrderId": "4zR9HFcEq8gM1tWUqPEUHc",  
      "price": "5.00000000",  
      "qty": "5.00000000",  
      "executedQty": "0.00000000",  
      "preventedQty": "0.00000000",  
      "quoteOrderQty": "0.00000000",  
      "cumulativeQuoteQty": "0.00000000",  
      "status": "NEW",  
      "timeInForce": "GTC",  
      "type": "LIMIT",  
      "side": "BUY",  
      "workingTime": 1741923284364,  
      "selfTradePreventionMode": "NONE"  
    }  
  },  
  "rateLimits":  
  [  
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

Response for an order which is part of an Order list:

```
{  
  "id": "56374b46-3061-486b-a311-89ee972eb648",  
  "status": 200,  
  "result":  
  {  
    "transactTime": 1741924229819,  
    "executionId": 60,  
    "amendedOrder":  
    {  
      "symbol": "BTUCSDT",  
      "orderId": 23,  
      "orderListId": 4,  
      "origClientOrderId": "my_pending_order",  
      "clientOrderId": "xbxXh5SSwaHS7oUEOCI88B",  
      "price": "1.00000000",  
      "qty": "5.00000000",  
      "executedQty": "0.00000000",  
      "preventedQty": "0.00000000",  
      "quoteOrderQty": "0.00000000",  
      "cumulativeQuoteQty": "0.00000000",  
      "status": "NEW",  
      "timeInForce": "GTC",  
      "type": "LIMIT",  
      "side": "BUY",  
      "workingTime": 1741924204920,  
      "selfTradePreventionMode": "NONE"  
    },  
    "listStatus":  
    {  
      "orderListId": 4,  
      "contingencyType": "OTO",  
      "listOrderStatus": "EXECUTING",  
      "listClientOrderId": "8nOGLLawudj1QoOiwbroRH",  
      "symbol": "BTCUSDT",  
      "orders":  
      [  
        {  
          "symbol": "BTCUSDT",  
          "orderId": 22,  
          "clientOrderId": "g04EWsjaackzedjC9wRkWD"  
        },  
        {  
          "symbol": "BTCUSDT",  
          "orderId": 23,  
          "clientOrderId": "xbxXh5SSwaHS7oUEOCI88B"  
        }  
      ]  
    }  
  },  
  "rateLimits":  
  [  
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

**Note:** The payloads above do not show all fields that can appear. Please refer to Conditional fields in Order Responses.