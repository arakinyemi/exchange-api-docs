### POST / Place algo order

The algo order includes trigger order, OCO order, conditional order, and trailing order.

Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Permission: Trade  

HTTP Request  

`POST /api/v5/trade/order-algo`

Request Examples

**Place Take Profit / Stop Loss Order**

```
POST /api/v5/trade/order-algo
{
    "instId":"BTC-USDT",
    "tdMode":"cash",
    "side":"buy",
    "ordType":"conditional",
    "sz":"2",
    "tpTriggerPx":"15",
    "tpOrdPx":"18"
}
```

**Place Trigger Order**

```
POST /api/v5/trade/order-algo
{
    "instId": "BTC-USDT",
    "tdMode": "cash",
    "side": "buy",
    "ordType": "trigger",
    "sz": "10",
    "triggerPx": "100",
    "orderPx": "-1"
}
```

**Place Trailing Stop Order**

```
POST /api/v5/trade/order-algo
{
    "instId": "BTC-USDT",
    "tdMode": "cash",
    "side": "buy",
    "ordType": "move_order_stop",
    "sz": "10",
    "callbackRatio": "0.05"
}
```

Request Parameters

| Parameter   | Type   | Required | Description                                    |
|-------------|--------|----------|------------------------------------------------|
| instId      | String | Yes      | Instrument ID, e.g. BTC-USDT                   |
| tdMode      | String | Yes      | Trade mode: cash                               |
| side        | String | Yes      | Order side: buy or sell                        |
| ordType     | String | Yes      | Order type: conditional, oco, trigger, move_order_stop |
| sz          | String | Yes      | Quantity to buy or sell                        |
| tag         | String | No       | Order tag (up to 16 characters)               |
| tgtCcy      | String | No       | Order quantity unit setting for sz            |
| algoClOrdId | String | No       | Client-supplied Algo ID                        |
