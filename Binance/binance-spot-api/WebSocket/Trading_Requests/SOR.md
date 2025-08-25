### SOR​

#### Place new order using SOR 

```
{  
  "id": "3a4437e2-41a3-4c19-897c-9cadc5dce8b6",  
  "method": "sor.order.place",  
  "params":  
  {  
    "symbol": "BTCUSDT",  
    "side": "BUY",  
    "type": "LIMIT",  
    "quantity": 0.5,  
    "timeInForce": "GTC",  
    "price": 31000,  
    "timestamp": 1687485436575,  
    "apiKey": "u5lgqJb97QWXWfgeV4cROuHbReSJM9rgQL0IvYcYc7BVeA5lpAqqc3a5p2OARIFk",  
    "signature": "fd301899567bc9472ce023392160cdc265ad8fcbbb67e0ea1b2af70a4b0cd9c7"  
  }  
}
```

Places an order using smart order routing (SOR).

This adds 1 order to the `EXCHANGE_MAX_ORDERS` filter and the `MAX_NUM_ORDERS` filter.

Read SOR FAQ to learn more.

**Weight:**
1

**Unfilled Order Count:**
1

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |
| `side` | ENUM | YES | `BUY` or `SELL` |
| `type` | ENUM | YES |  |
| `timeInForce` | ENUM | NO | Applicable only to `LIMIT` order type |
| `price` | DECIMAL | NO | Applicable only to `LIMIT` order type |
| `quantity` | DECIMAL | YES |  |
| `newClientOrderId` | STRING | NO | Arbitrary unique ID among open orders. Automatically generated if not sent |
| `newOrderRespType` | ENUM | NO | Select response format: `ACK`, `RESULT`, `FULL`.  `MARKET` and `LIMIT` orders use `FULL` by default. |
| `icebergQty` | DECIMAL | NO |  |
| `strategyId` | LONG | NO | Arbitrary numeric value identifying the order within an order strategy. |
| `strategyType` | INT | NO | Arbitrary numeric value identifying the order strategy.  Values smaller than `1000000` are reserved and cannot be used. |
| `selfTradePreventionMode` | ENUM | NO | The allowed enums is dependent on what is configured on the symbol. The possible supported values are: STP Modes. |
| `apiKey` | STRING | YES |  |
| `timestamp` | LONG | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |

**Note:** `sor.order.place` only supports `LIMIT` and `MARKET` orders. `quoteOrderQty` is not supported.

**Data Source:**
Matching Engine

**Response:**

```
{  
  "id": "3a4437e2-41a3-4c19-897c-9cadc5dce8b6",  
  "status": 200,  
  "result": [  
    {  
      "symbol": "BTCUSDT",  
      "orderId": 2,  
      "orderListId": -1,  
      "clientOrderId": "sBI1KM6nNtOfj5tccZSKly",  
      "transactTime": 1689149087774,  
      "price": "31000.00000000",  
      "origQty": "0.50000000",  
      "executedQty": "0.50000000",  
      "origQuoteOrderQty": "0.000000",  
      "cummulativeQuoteQty": "14000.00000000",  
      "status": "FILLED",  
      "timeInForce": "GTC",  
      "type": "LIMIT",  
      "side": "BUY",  
      "workingTime": 1689149087774,  
      "fills": [  
        {  
          "matchType": "ONE_PARTY_TRADE_REPORT",  
          "price": "28000.00000000",  
          "qty": "0.50000000",  
          "commission": "0.00000000",  
          "commissionAsset": "BTC",  
          "tradeId": -1,  
          "allocId": 0  
        }  
      ],  
      "workingFloor": "SOR",  
      "selfTradePreventionMode": "NONE",  
      "usedSor": true  
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

#### Test new order using SOR 

```
{  
  "id": "3a4437e2-41a3-4c19-897c-9cadc5dce8b6",  
  "method": "sor.order.test",  
  "params":  
  {  
    "symbol": "BTCUSDT",  
    "side": "BUY",  
    "type": "LIMIT",  
    "quantity": 0.1,  
    "timeInForce": "GTC",  
    "price": 0.1,  
    "timestamp": 1687485436575,  
    "apiKey": "u5lgqJb97QWXWfgeV4cROuHbReSJM9rgQL0IvYcYc7BVeA5lpAqqc3a5p2OARIFk",  
    "signature": "fd301899567bc9472ce023392160cdc265ad8fcbbb67e0ea1b2af70a4b0cd9c7"  
  }  
}
```

Test new order creation and signature/recvWindow using smart order routing (SOR).
Creates and validates a new order but does not send it into the matching engine.

**Weight:**

| Condition | Request Weight |
| --- | --- |
| Without `computeCommissionRates` | 1 |
| With `computeCommissionRates` | 20 |

**Parameters:**

In addition to all parameters accepted by [`sor.order.place`](/docs/binance-spot-api-docs/websocket-api/trading-requests#place-new-order-using-sor-trade),
the following optional parameters are also accepted:

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `computeCommissionRates` | BOOLEAN | NO | Default: `false` |

**Data Source:**
Memory

**Response:**

Without `computeCommissionRates`:

```json
{  
  "id": "3a4437e2-41a3-4c19-897c-9cadc5dce8b6",  
  "status": 200,  
  "result": {},  
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

With `computeCommissionRates`:

```json
{  
  "id": "3a4437e2-41a3-4c19-897c-9cadc5dce8b6",  
  "status": 200,  
  "result": {  
    "standardCommissionForOrder": {                //Commission rates for the order depending on its role (e.g. maker or taker)  
      "maker": "0.00000112",  
      "taker": "0.00000114"  
    },  
    "taxCommissionForOrder": {                     //Tax deduction rates for the order depending on its role (e.g. maker or taker)  
      "maker": "0.00000112",  
      "taker": "0.00000114"  
    },  
    "discount": {                                  //Discount on standard commissions when paying in BNB.  
      "enabledForAccount": true,  
      "enabledForSymbol": true,  
      "discountAsset": "BNB",  
      "discount": "0.25"                           //Standard commission is reduced by this rate when paying in BNB.  
    }  
  },  
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
