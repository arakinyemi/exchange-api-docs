### WebSocket
The following parameters are set in the request

| Parameter | Type   | Required | Description                                                          |
|-----------|--------|----------|----------------------------------------------------------------------|
| expTime   | String | No       | Request effective deadline. Unix timestamp in milliseconds, e.g., 1597026383085 |

The following endpoints are supported:

Place order

Place multiple orders

Amend order

Amend multiple orders

Request Example

```json
{
    "id": "1512",
    "op": "order",
    "expTime":"1597026383085",  // request effective deadline
    "args": [{
        "side": "buy",
        "instId": "BTC-USDT",
        "tdMode": "isolated",
        "ordType": "market",
        "sz": "100"
    }]
}
```
