### Cancel and replace order

```
{  
  "id": "99de1036-b5e2-4e0f-9b5c-13d751c93a1a",  
  "method": "order.cancelReplace",  
  "params": {  
    "symbol": "BTCUSDT",  
    "cancelReplaceMode": "ALLOW_FAILURE",  
    "cancelOrigClientOrderId": "4d96324ff9d44481926157",  
    "side": "SELL",  
    "type": "LIMIT",  
    "timeInForce": "GTC",  
    "price": "23416.10000000",  
    "quantity": "0.00847000",  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "7028fdc187868754d25e42c37ccfa5ba2bab1d180ad55d4c3a7e2de643943dc5",  
    "timestamp": 1660813156900  
  }  
}
```

Cancel an existing order and immediately place a new order instead of the canceled one.

A new order that was not attempted (i.e. when `newOrderResult: NOT_ATTEMPTED`), will still increase the unfilled order count by 1.

**Weight:**
1

**Unfilled Order Count:**
1

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `cancelReplaceMode` | ENUM | YES |  |
| `cancelOrderId` | LONG | YES | Cancel order by `orderId` |
| `cancelOrigClientOrderId` | STRING | Cancel order by `clientOrderId` |
| `cancelNewClientOrderId` | STRING | NO | New ID for the canceled order. Automatically generated if not sent |
| `side` | ENUM | YES | `BUY` or `SELL` |
| `type` | ENUM | YES |  |
| `timeInForce` | ENUM | NO \* |  |
| `price` | DECIMAL | NO \* |  |
| `quantity` | DECIMAL | NO \* |  |
| `quoteOrderQty` | DECIMAL | NO \* |  |
| `newClientOrderId` | STRING | NO | Arbitrary unique ID among open orders. Automatically generated if not sent |
| `newOrderRespType` | ENUM | NO | Select response format: `ACK`, `RESULT`, `FULL`.  `MARKET` and `LIMIT` orders produce `FULL` response by default, other order types default to `ACK`. |
| `stopPrice` | DECIMAL | NO \* |  |
| `trailingDelta` | DECIMAL | NO \* | See Trailing Stop order FAQ |
| `icebergQty` | DECIMAL | NO |  |
| `strategyId` | LONG | NO | Arbitrary numeric value identifying the order within an order strategy. |
| `strategyType` | INT | NO | Arbitrary numeric value identifying the order strategy.  Values smaller than 1000000 are reserved and cannot be used. |
| `selfTradePreventionMode` | ENUM | NO | The allowed enums is dependent on what is configured on the symbol.  Supported values: STP Modes. |
| `cancelRestrictions` | ENUM | NO | Supported values:  `ONLY_NEW` - Cancel will succeed if the order status is `NEW`.  `ONLY_PARTIALLY_FILLED` - Cancel will succeed if order status is `PARTIALLY_FILLED`. For more information please refer to [Regarding `cancelRestrictions`](/docs/binance-spot-api-docs/websocket-api/trading-requests#regarding-cancelrestrictions). |
| `apiKey` | STRING | YES |  |
| `orderRateLimitExceededMode` | ENUM | NO | Supported values:   `DO_NOTHING` (default)- will only attempt to cancel the order if account has not exceeded the unfilled order rate limit  `CANCEL_ONLY` - will always cancel the order. |
| `pegPriceType` | ENUM | NO | `PRIMARY_PEG` or `MARKET_PEG`.  See Pegged Orders" |
| `pegOffsetValue` | INT | NO | Price level to peg the price to (max: 100)   See Pegged Orders |
| `pegOffsetType` | ENUM | NO | Only `PRICE_LEVEL` is supported See Pegged Orders |
| `recvWindow` | LONG | NO | The value cannot be greater than 60000 |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

Similar to the [`order.place`](/docs/binance-spot-api-docs/websocket-api/trading-requests#place-new-order-trade) request,
additional mandatory parameters (\*) are determined by the new order [`type`](/docs/binance-spot-api-docs/websocket-api/trading-requests#order-type).

Available `cancelReplaceMode` options:

* `STOP_ON_FAILURE` – if cancellation request fails, new order placement will not be attempted.
* `ALLOW_FAILURE` – new order placement will be attempted even if the cancel request fails.

| Request | | | Response | | |
| --- | --- | --- | --- | --- | --- |
| `cancelReplaceMode` | `orderRateLimitExceededMode` | Unfilled Order Count | `cancelResult` | `newOrderResult` | `status` |
| `STOP_ON_FAILURE` | `DO_NOTHING` | Within Limits | ✅ `SUCCESS` | ✅ `SUCCESS` | `200` |
| | | |❌ `FAILURE` | ➖ `NOT_ATTEMPTED` | `400` |
| | | |✅ `SUCCESS` | ❌ `FAILURE` | `409` |
| Exceeds Limits | | | ✅ `SUCCESS` | ✅ `SUCCESS` | N/A |
| | | | ❌ `FAILURE` | ➖ `NOT_ATTEMPTED` | N/A |
| | | |✅ `SUCCESS` | ❌ `FAILURE` | N/A |
| `CANCEL_ONLY` | | Within Limits | ✅ `SUCCESS` | ✅ `SUCCESS` | `200` |
| | | |❌ `FAILURE` | ➖ `NOT_ATTEMPTED` | `400` |
| | | |✅ `SUCCESS` | ❌ `FAILURE` | `409` |
| | | Exceeds Limits | ❌ `FAILURE` | ➖ `NOT_ATTEMPTED` | `429` |
| | | ✅ `SUCCESS` | ❌ `FAILURE` | `429` |
| `ALLOW_FAILURE` | `DO_NOTHING` | Within Limits | ✅ `SUCCESS` | ✅ `SUCCESS` | `200` |
| | | |❌ `FAILURE` | ❌ `FAILURE` | `400` |
| | | |❌ `FAILURE` | ✅ `SUCCESS` | `409` |
|  | ||✅ `SUCCESS` | ❌ `FAILURE` | `409` |
| | | Exceeds Limits | ✅ `SUCCESS` | ✅ `SUCCESS` | N/A |
| | | |❌ `FAILURE` | ❌ `FAILURE` | N/A |
| | | |❌ `FAILURE` | ✅ `SUCCESS` | N/A |
| | | |✅ `SUCCESS` | ❌ `FAILURE` | N/A |
| `CANCEL_ONLY` | Within Limits | ✅ `SUCCESS` | ✅ `SUCCESS` | `200` |
| | | |❌ `FAILURE` | ❌ `FAILURE` | `400` |
| | | |❌ `FAILURE` | ✅ `SUCCESS` | `409` |
| | | |✅ `SUCCESS` | ❌ `FAILURE` | `409` |
| | | Exceeds Limits | ✅ `SUCCESS` | ✅ `SUCCESS` | `N/A` |
| | | |❌ `FAILURE` | ❌ `FAILURE` | `400` |
| | | |❌ `FAILURE` | ✅ `SUCCESS` | N/A |
| | | | ✅ `SUCCESS` | ❌ `FAILURE` | `409` |

Notes:

* If both `cancelOrderId` and `cancelOrigClientOrderId` parameters are provided, the `cancelOrderId` is searched first, then the `cancelOrigClientOrderId` from that result is checked against that order. If both conditions are not met the request will be rejected.
* `cancelNewClientOrderId` will replace `clientOrderId` of the canceled order, freeing it up for new orders.
* `newClientOrderId` specifies `clientOrderId` value for the placed order.

  A new order with the same `clientOrderId` is accepted only when the previous one is filled or expired.

  The new order can reuse old `clientOrderId` of the canceled order.
* This cancel-replace operation is **not transactional**.

  If one operation succeeds but the other one fails, the successful operation is still executed.

  For example, in `STOP_ON_FAILURE` mode, if the new order placement fails, the old order is still canceled.
* Filters and order count limits are evaluated before cancellation and order placement occurs.
* If new order placement is not attempted, your order count is still incremented.
* Like [`order.cancel`](/docs/binance-spot-api-docs/websocket-api/trading-requests#cancel-order-trade), if you cancel an individual order from an order list, the entire order list is canceled.
* The performance for canceling an order (single cancel or as part of a cancel-replace) is always better when only `orderId` is sent. Sending `origClientOrderId` or both `orderId` + `origClientOrderId` will be slower.

**Data Source:**
Matching Engine

**Response:**

If both cancel and placement succeed, you get the following response with `"status": 200`:

```
{  
  "id": "99de1036-b5e2-4e0f-9b5c-13d751c93a1a",  
  "status": 200,  
  "result": {  
    "cancelResult": "SUCCESS",  
    "newOrderResult": "SUCCESS",  
    // Format is identical to "order.cancel" format.  
    // Some fields are optional and are included only for orders that set them.  
    "cancelResponse": {  
      "symbol": "BTCUSDT",  
      "origClientOrderId": "4d96324ff9d44481926157",  // cancelOrigClientOrderId from request  
      "orderId": 125690984230,  
      "orderListId": -1,  
      "clientOrderId": "91fe37ce9e69c90d6358c0",      // cancelNewClientOrderId from request  
      "transactTime": 1684804350068,  
      "price": "23450.00000000",  
      "origQty": "0.00847000",  
      "executedQty": "0.00001000",  
      "origQuoteOrderQty": "0.000000",  
      "cummulativeQuoteQty": "0.23450000",  
      "status": "CANCELED",  
      "timeInForce": "GTC",  
      "type": "LIMIT",  
      "side": "SELL",  
      "selfTradePreventionMode": "NONE"  
    },  
    // Format is identical to "order.place" format, affected by "newOrderRespType".  
    // Some fields are optional and are included only for orders that set them.  
    "newOrderResponse": {  
      "symbol": "BTCUSDT",  
      "orderId": 12569099453,  
      "orderListId": -1,  
      "clientOrderId": "bX5wROblo6YeDwa9iTLeyY",      // newClientOrderId from request  
      "transactTime": 1660813156959,  
      "price": "23416.10000000",  
      "origQty": "0.00847000",  
      "executedQty": "0.00000000",  
      "origQuoteOrderQty": "0.000000",  
      "cummulativeQuoteQty": "0.00000000",  
      "status": "NEW",  
      "timeInForce": "GTC",  
      "type": "LIMIT",  
      "side": "SELL",  
      "selfTradePreventionMode": "NONE"  
    }  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "SECOND",  
      "intervalNum": 10,  
      "limit": 50,  
      "count": 1  
    },  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "DAY",  
      "intervalNum": 1,  
      "limit": 160000,  
      "count": 1  
    },  
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

In `STOP_ON_FAILURE` mode, failed order cancellation prevents new order from being placed
and returns the following response with `"status": 400`:

```
{  
  "id": "27e1bf9f-0539-4fb0-85c6-06183d36f66c",  
  "status": 400,  
  "error": {  
    "code": -2022,  
    "msg": "Order cancel-replace failed.",  
    "data": {  
      "cancelResult": "FAILURE",  
      "newOrderResult": "NOT_ATTEMPTED",  
      "cancelResponse": {  
        "code": -2011,  
        "msg": "Unknown order sent."  
      },  
      "newOrderResponse": null  
    }  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "SECOND",  
      "intervalNum": 10,  
      "limit": 50,  
      "count": 1  
    },  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "DAY",  
      "intervalNum": 1,  
      "limit": 160000,  
      "count": 1  
    },  
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

If cancel-replace mode allows failure and one of the operations fails,
you get a response with `"status": 409`,
and the `"data"` field detailing which operation succeeded, which failed, and why:

```
{  
  "id": "b220edfe-f3c4-4a3a-9d13-b35473783a25",  
  "status": 409,  
  "error": {  
    "code": -2021,  
    "msg": "Order cancel-replace partially failed.",  
    "data": {  
      "cancelResult": "SUCCESS",  
      "newOrderResult": "FAILURE",  
      "cancelResponse": {  
        "symbol": "BTCUSDT",  
        "origClientOrderId": "4d96324ff9d44481926157",  
        "orderId": 125690984230,  
        "orderListId": -1,  
        "clientOrderId": "91fe37ce9e69c90d6358c0",  
        "transactTime": 1684804350068,  
        "price": "23450.00000000",  
        "origQty": "0.00847000",  
        "executedQty": "0.00001000",  
        "origQuoteOrderQty": "0.000000",  
        "cummulativeQuoteQty": "0.23450000",  
        "status": "CANCELED",  
        "timeInForce": "GTC",  
        "type": "LIMIT",  
        "side": "SELL",  
        "selfTradePreventionMode": "NONE"  
      },  
      "newOrderResponse": {  
        "code": -2010,  
        "msg": "Order would immediately match and take."  
      }  
    }  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "SECOND",  
      "intervalNum": 10,  
      "limit": 50,  
      "count": 1  
    },  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "DAY",  
      "intervalNum": 1,  
      "limit": 160000,  
      "count": 1  
    },  
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

```
{  
  "id": "ce641763-ff74-41ac-b9f7-db7cbe5e93b1",  
  "status": 409,  
  "error": {  
    "code": -2021,  
    "msg": "Order cancel-replace partially failed.",  
    "data": {  
      "cancelResult": "FAILURE",  
      "newOrderResult": "SUCCESS",  
      "cancelResponse": {  
        "code": -2011,  
        "msg": "Unknown order sent."  
      },  
      "newOrderResponse": {  
        "symbol": "BTCUSDT",  
        "orderId": 12569099453,  
        "orderListId": -1,  
        "clientOrderId": "bX5wROblo6YeDwa9iTLeyY",  
        "transactTime": 1660813156959,  
        "price": "23416.10000000",  
        "origQty": "0.00847000",  
        "executedQty": "0.00000000",  
        "origQuoteOrderQty": "0.000000",  
        "cummulativeQuoteQty": "0.00000000",  
        "status": "NEW",  
        "timeInForce": "GTC",  
        "type": "LIMIT",  
        "side": "SELL",  
        "workingTime": 1669693344508,  
        "fills": [],  
        "selfTradePreventionMode": "NONE"  
      }  
    }  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "SECOND",  
      "intervalNum": 10,  
      "limit": 50,  
      "count": 1  
    },  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "DAY",  
      "intervalNum": 1,  
      "limit": 160000,  
      "count": 1  
    },  
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

If both operations fail, response will have `"status": 400`:

```
{  
  "id": "3b3ac45c-1002-4c7d-88e8-630c408ecd87",  
  "status": 400,  
  "error": {  
    "code": -2022,  
    "msg": "Order cancel-replace failed.",  
    "data": {  
      "cancelResult": "FAILURE",  
      "newOrderResult": "FAILURE",  
      "cancelResponse": {  
        "code": -2011,  
        "msg": "Unknown order sent."  
      },  
      "newOrderResponse": {  
        "code": -2010,  
        "msg": "Order would immediately match and take."  
      }  
    }  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "SECOND",  
      "intervalNum": 10,  
      "limit": 50,  
      "count": 1  
    },  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "DAY",  
      "intervalNum": 1,  
      "limit": 160000,  
      "count": 1  
    },  
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

If `orderRateLimitExceededMode` is `DO_NOTHING` regardless of `cancelReplaceMode`, and you have exceeded your unfilled order count, you will get status `429` with the following error:

```
{  
  "id": "3b3ac45c-1002-4c7d-88e8-630c408ecd87",  
  "status": 429,  
  "error": {  
    "code": -1015,  
    "msg": "Too many new orders; current limit is 50 orders per 10 SECOND."  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "SECOND",  
      "intervalNum": 10,  
      "limit": 50,  
      "count": 50  
    },  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "DAY",  
      "intervalNum": 1,  
      "limit": 160000,  
      "count": 50  
    },  
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

If `orderRateLimitExceededMode` is `CANCEL_ONLY` regardless of `cancelReplaceMode`, and you have exceeded your unfilled order count, you will get status `409` with the following error:

```
{  
  "id": "3b3ac45c-1002-4c7d-88e8-630c408ecd87",  
  "status": 409,  
  "error": {  
    "code": -2021,  
    "msg": "Order cancel-replace partially failed.",  
    "data": {  
      "cancelResult": "SUCCESS",  
      "newOrderResult": "FAILURE",  
      "cancelResponse": {  
        "symbol": "LTCBNB",  
        "origClientOrderId": "GKt5zzfOxRDSQLveDYCTkc",  
        "orderId": 64,  
        "orderListId": -1,  
        "clientOrderId": "loehOJF3FjoreUBDmv739R",  
        "transactTime": 1715779007228,  
        "price": "1.00",  
        "origQty": "10.00000000",  
        "executedQty": "0.00000000",  
        "origQuoteOrderQty": "0.000000",  
        "cummulativeQuoteQty": "0.00",  
        "status": "CANCELED",  
        "timeInForce": "GTC",  
        "type": "LIMIT",  
        "side": "SELL",  
        "selfTradePreventionMode": "NONE"  
      },  
      "newOrderResponse": {  
        "code": -1015,  
        "msg": "Too many new orders; current limit is 50 orders per 10 SECOND."  
      }  
    }  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "SECOND",  
      "intervalNum": 10,  
      "limit": 50,  
      "count": 50  
    },  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "DAY",  
      "intervalNum": 1,  
      "limit": 160000,  
      "count": 50  
    },  
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