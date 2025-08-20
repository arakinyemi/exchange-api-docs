# Order Price Limit

Subscribe to Get Order Price Limit.

- For derivative trading order price limit, refer to announcement
- For spot trading order price limit, refer to announcement

**Push frequency:** 300ms

### Topic:
`priceLimit.{symbol}`

### Response Parameters

| Parameter | Type | Comments |
|-----------|------|----------|
| topic | string | Topic name |
| ts | number | The timestamp (ms) that the system generates the data |
| data | array | Object. |
| > symbol | string | Symbol name |
| > buyLmt | string | Highest Bid Price |
| > sellLmt | string | Lowest Ask Price |

### Subscribe Example

```json
{"op": "subscribe", "args": ["priceLimit.BTCUSDT"]}
```

### Response Example

```json
{
    "topic": "priceLimit.BTCUSDT",
    "data": {
        "symbol": "BTCUSDT",
        "buyLmt": "114450.00",
        "sellLmt": "103550.00"
    },
    "ts": 1750059683782
}
```