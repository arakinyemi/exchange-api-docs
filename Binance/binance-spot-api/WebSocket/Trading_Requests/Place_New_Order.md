### Place new order 

```json
{  
  "id": "56374a46-3061-486b-a311-99ee972eb648",  
  "method": "order.place",  
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

Send in a new order.

This adds 1 order to the `EXCHANGE_MAX_ORDERS` filter and the `MAX_NUM_ORDERS` filter.

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
| `timeInForce` | ENUM | NO \* |  |
| `price` | DECIMAL | NO \* |  |
| `quantity` | DECIMAL | NO \* |  |
| `quoteOrderQty` | DECIMAL | NO \* |  |
| `newClientOrderId` | STRING | NO | Arbitrary unique ID among open orders. Automatically generated if not sent |
| `newOrderRespType` | ENUM | NO | Select response format: `ACK`, `RESULT`, `FULL`.  `MARKET` and `LIMIT` orders use `FULL` by default, other order types default to `ACK`. |
| `stopPrice` | DECIMAL | NO \* |  |
| `trailingDelta` | INT | NO \* | See Trailing Stop order FAQ |
| `icebergQty` | DECIMAL | NO |  |
| `strategyId` | LONG | NO | Arbitrary numeric value identifying the order within an order strategy. |
| `strategyType` | INT | NO | Arbitrary numeric value identifying the order strategy.  Values smaller than `1000000` are reserved and cannot be used. |
| `selfTradePreventionMode` | ENUM | NO | The allowed enums is dependent on what is configured on the symbol. Supported values: STP Modes |
| `pegPriceType` | ENUM | NO | `PRIMARY_PEG` or `MARKET_PEG`   See Pegged Orders |
| `pegOffsetValue` | INT | NO | Price level to peg the price to (max: 100)   See Pegged Orders |
| `pegOffsetType` | ENUM | NO | Only `PRICE_LEVEL` is supported   See Pegged Orders |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

Certain parameters (\*) become mandatory based on the order `type`:

| Order `type` | Mandatory parameters |
| --- | --- |
| `LIMIT` | * `timeInForce` * `price` * `quantity` |
| `LIMIT_MAKER` | * `price` * `quantity` |
| `MARKET` | * `quantity` or `quoteOrderQty` |
| `STOP_LOSS` | * `quantity` * `stopPrice` or `trailingDelta` |
| `STOP_LOSS_LIMIT` | * `timeInForce` * `price` * `quantity` * `stopPrice` or `trailingDelta` |
| `TAKE_PROFIT` | * `quantity` * `stopPrice` or `trailingDelta` |
| `TAKE_PROFIT_LIMIT` | * `timeInForce` * `price` * `quantity` * `stopPrice` or `trailingDelta` |

Supported order types:

| Order `type` | Description |
| --- | --- |
| `LIMIT` | Buy or sell `quantity` at the specified `price` or better. |
| `LIMIT_MAKER` | `LIMIT` order that will be rejected if it immediately matches and trades as a taker.  This order type is also known as a POST-ONLY order. |
| `MARKET` | Buy or sell at the best available market price.   * `MARKET` order with `quantity` parameter   specifies the amount of the *base asset* you want to buy or sell.   Actually executed quantity of the quote asset will be determined by available market liquidity.  E.g., a MARKET BUY order on BTCUSDT for `"quantity": "0.1000"`   specifies that you want to buy 0.1 BTC at the best available price.   If there is not enough BTC at the best price, keep buying at the next best price,   until either your order is filled, or you run out of USDT, or market runs out of BTC. * `MARKET` order with `quoteOrderQty` parameter   specifies the amount of the *quote asset* you want to spend (when buying) or receive (when selling).   Actually executed quantity of the base asset will be determined by available market liquidity.  E.g., a MARKET BUY on BTCUSDT for `"quoteOrderQty": "100.00"`   specifies that you want to buy as much BTC as you can for 100 USDT at the best available price.   Similarly, a SELL order will sell as much available BTC as needed for you to receive 100 USDT   (before commission). |
| `STOP_LOSS` | Execute a `MARKET` order for given `quantity` when specified conditions are met.  I.e., when `stopPrice` is reached, or when `trailingDelta` is activated. |
| `STOP_LOSS_LIMIT` | Place a `LIMIT` order with given parameters when specified conditions are met. |
| `TAKE_PROFIT` | Like `STOP_LOSS` but activates when market price moves in the favorable direction. |
| `TAKE_PROFIT_LIMIT` | Like `STOP_LOSS_LIMIT` but activates when market price moves in the favorable direction. |

Notes on using parameters for Pegged Orders:

* These parameters are allowed for `LIMIT`, `LIMIT_MAKER`, `STOP_LOSS_LIMIT`, `TAKE_PROFIT_LIMIT` orders.
* If `pegPriceType` is specified, `price` becomes optional. Otherwise, it is still mandatory.
* `pegPriceType=PRIMARY_PEG` means the primary peg, that is the best price on the same side of the order book as your order.
* `pegPriceType=MARKET_PEG` means the market peg, that is the best price on the opposite side of the order book from your order.
* Use `pegOffsetType` and `pegOffsetValue` to request a price level other than the best one. These parameters must be specified together.

Available `timeInForce` options,
setting how long the order should be active before expiration:

| TIF | Description |
| --- | --- |
| `GTC` | **Good 'til Canceled** – the order will remain on the book until you cancel it, or the order is completely filled. |
| `IOC` | **Immediate or Cancel** – the order will be filled for as much as possible, the unfilled quantity immediately expires. |
| `FOK` | **Fill or Kill** – the order will expire unless it cannot be immediately filled for the entire quantity. |

Notes:

* `newClientOrderId` specifies `clientOrderId` value for the order.

  A new order with the same `clientOrderId` is accepted only when the previous one is filled or expired.
* Any `LIMIT` or `LIMIT_MAKER` order can be made into an iceberg order by specifying the `icebergQty`.

  An order with an `icebergQty` must have `timeInForce` set to `GTC`.
* Trigger order price rules for `STOP_LOSS`/`TAKE_PROFIT` orders:

  * `stopPrice` must be above market price: `STOP_LOSS BUY`, `TAKE_PROFIT SELL`
  * `stopPrice` must be below market price: `STOP_LOSS SELL`, `TAKE_PROFIT BUY`
* `MARKET` orders using `quoteOrderQty` follow [`LOT_SIZE`](/docs/binance-spot-api-docs/filters#lot_size) filter rules.

  The order will execute a quantity that has notional value as close as possible to requested `quoteOrderQty`.

**Data Source:**
Matching Engine

**Response:**

Response format is selected by using the `newOrderRespType` parameter.

`ACK` response type:

```json
{  
  "id": "56374a46-3061-486b-a311-99ee972eb648",  
  "status": 200,  
  "result": {  
    "symbol": "BTCUSDT",  
    "orderId": 12569099453,  
    "orderListId": -1, // always -1 for singular orders  
    "clientOrderId": "4d96324ff9d44481926157ec08158a40",  
    "transactTime": 1660801715639  
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

`RESULT` response type:

```json
{  
  "id": "56374a46-3061-486b-a311-99ee972eb648",  
  "status": 200,  
  "result": {  
    "symbol": "BTCUSDT",  
    "orderId": 12569099453,  
    "orderListId": -1, // always -1 for singular orders  
    "clientOrderId": "4d96324ff9d44481926157ec08158a40",  
    "transactTime": 1660801715639,  
    "price": "23416.10000000",  
    "origQty": "0.00847000",  
    "executedQty": "0.00000000",  
    "origQuoteOrderQty": "0.000000",  
    "cummulativeQuoteQty": "0.00000000",  
    "status": "NEW",  
    "timeInForce": "GTC",  
    "type": "LIMIT",  
    "side": "SELL",  
    "workingTime": 1660801715639,  
    "selfTradePreventionMode": "NONE"  
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
      "limit": 6000  
,  
      "count": 1  
    }  
  ]  
}
```

`FULL` response type:

```json
{  
  "id": "56374a46-3061-486b-a311-99ee972eb648",  
  "status": 200,  
  "result": {  
    "symbol": "BTCUSDT",  
    "orderId": 12569099453,  
    "orderListId": -1,  
    "clientOrderId": "4d96324ff9d44481926157ec08158a40",  
    "transactTime": 1660801715793,  
    "price": "23416.10000000",  
    "origQty": "0.00847000",  
    "executedQty": "0.00847000",  
    "origQuoteOrderQty": "0.000000",  
    "cummulativeQuoteQty": "198.33521500",  
    "status": "FILLED",  
    "timeInForce": "GTC",  
    "type": "LIMIT",  
    "side": "SELL",  
    "workingTime": 1660801715793,  
    // FULL response is identical to RESULT response, with the same optional fields  
    // based on the order type and parameters. FULL response additionally includes  
    // the list of trades which immediately filled the order.  
    "fills": [  
      {  
        "price": "23416.10000000",  
        "qty": "0.00635000",  
        "commission": "0.000000",  
        "commissionAsset": "BNB",  
        "tradeId": 1650422481  
      },  
      {  
        "price": "23416.50000000",  
        "qty": "0.00212000",  
        "commission": "0.000000",  
        "commissionAsset": "BNB",  
        "tradeId": 1650422482  
      }  
    ]  
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

**Conditional fields in Order Responses**

There are fields in the order responses (e.g. order placement, order query, order cancellation) that appear only if certain conditions are met.

These fields can apply to Order lists.

The fields are listed below:

| Field | Description | Visibility conditions | Examples |
| --- | --- | --- | --- |
| `icebergQty` | Quantity for the iceberg order | Appears only if the parameter `icebergQty` was sent in the request. | `"icebergQty": "0.00000000"` |
| `preventedMatchId` | When used in combination with `symbol`, can be used to query a prevented match. | Appears only if the order expired due to STP. | `"preventedMatchId": 0` |
| `preventedQuantity` | Order quantity that expired due to STP | Appears only if the order expired due to STP. | `"preventedQuantity": "1.200000"` |
| `stopPrice` | Price when the algorithmic order will be triggered | Appears for `STOP_LOSS`. `TAKE_PROFIT`, `STOP_LOSS_LIMIT` and `TAKE_PROFIT_LIMIT` orders. | `"stopPrice": "23500.00000000"` |
| `strategyId` | Can be used to label an order that's part of an order strategy. | Appears if the parameter was populated in the request. | `"strategyId": 37463720` |
| `strategyType` | Can be used to label an order that is using an order strategy. | Appears if the parameter was populated in the request. | `"strategyType": 1000000` |
| `trailingDelta` | Delta price change required before order activation | Appears for Trailing Stop Orders. | `"trailingDelta": 10` |
| `trailingTime` | Time when the trailing order is now active and tracking price changes | Appears only for Trailing Stop Orders. | `"trailingTime": -1` |
| `usedSor` | Field that determines whether order used SOR | Appears when placing orders using SOR | `"usedSor": true` |
| `workingFloor` | Field that determines whether the order is being filled by the SOR or by the order book the order was submitted to. | Appears when placing orders using SOR | `"workingFloor": "SOR"` |
| `pegPriceType` | Price peg type | Only for pegged orders | `"pegPriceType": "PRIMARY_PEG"` |
| `pegOffsetType` | Price peg offset type | Only for pegged orders, if requested | `"pegOffsetType": "PRICE_LEVEL"` |
| `pegOffsetValue` | Price peg offset value | Only for pegged orders, if requested | `"pegOffsetValue": 5` |
| `peggedPrice` | Current price order is pegged at | Only for pegged orders, once determined | `"peggedPrice": "87523.83710000"` |