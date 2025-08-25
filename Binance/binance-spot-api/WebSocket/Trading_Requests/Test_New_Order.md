### Test new order 

```
{  
  "id": "6ffebe91-01d9-43ac-be99-57cf062e0e30",  
  "method": "order.test",  
  "params": {  
    "symbol": "BTCUSDT",  
    "side": "SELL",  
    "type": "LIMIT",  
    "timeInForce": "GTC",  
    "price": "23416.10000000",  
    "quantity": "0.00847000",  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "15af09e41c36f3cc61378c2fbe2c33719a03dd5eba8d0f9206fbda44de717c88",  
    "timestamp": 1660801715431  
  }  
}
```

Test order placement.

Validates new order parameters and verifies your signature
but does not send the order into the matching engine.

**Weight:**

| Condition | Request Weight |
| --- | --- |
| Without `computeCommissionRates` | 1 |
| With `computeCommissionRates` | 20 |

**Parameters:**

In addition to all parameters accepted by [`order.place`](/docs/binance-spot-api-docs/websocket-api/trading-requests#place-new-order-trade),
the following optional parameters are also accepted:

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `computeCommissionRates` | BOOLEAN | NO | Default: `false`   See Commissions FAQ to learn more. |

**Data Source:**
Memory

**Response:**

Without `computeCommissionRates`:

```
{  
  "id": "6ffebe91-01d9-43ac-be99-57cf062e0e30",  
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

```
{  
  "id": "6ffebe91-01d9-43ac-be99-57cf062e0e30",  
  "status": 200,  
  "result": {  
    "standardCommissionForOrder": {           //Standard commission rates on trades from the order.  
      "maker": "0.00000112",  
      "taker": "0.00000114"  
    },  
    "specialCommissionForOrder": {           //Special commission rates on trades from the order.  
      "maker": "0.05000000",  
      "taker": "0.06000000"  
    },  
    "taxCommissionForOrder": {                //Tax commission rates for trades from the order  
      "maker": "0.00000112",  
      "taker": "0.00000114"  
    },  
    "discount": {                             //Discount on standard commissions when paying in BNB.  
      "enabledForAccount": true,  
      "enabledForSymbol": true,  
      "discountAsset": "BNB",  
      "discount": "0.25000000"                //Standard commission is reduced by this rate when paying in BNB.  
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