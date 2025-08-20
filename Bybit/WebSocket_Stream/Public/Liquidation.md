# Liquidation (deprecated)

**Deprecated liquidation stream, please move to All Liquidation**

Subscribe to the liquidation stream. Pushes at most one order per second per symbol. As such, this feed does not push all liquidations that occur on Bybit.

**Push frequency:** 1s

### Topic:
`liquidation.{symbol}` e.g., `liquidation.BTCUSDT`

### Response Parameters

| Parameter | Type | Comments |
|-----------|------|----------|
| topic | string | Topic name |
| type | string | Data type. snapshot |
| ts | number | The timestamp (ms) that the system generates the data |
| data | Object |  |
| > updateTime | number | The updated timestamp (ms) |
| > symbol | string | Symbol name |
| > side | string | Position side. Buy,Sell. When you receive a Buy update, this means that a long position has been liquidated |
| > size | string | Executed size |
| > price | string | Bankruptcy price |

### Subscribe Example

```python
from pybit.unified_trading import WebSocket
from time import sleep
ws = WebSocket(
    testnet=True,
    channel_type="linear",
)
def handle_message(message):
    print(message)
ws.liquidation_stream(
    symbol="BTCUSDT",
    callback=handle_message
)
while True:
    sleep(1)
```

### Response Example

```json
{
    "topic": "liquidation.BTCUSDT",
    "type": "snapshot",
    "ts": 1703485237953,
    "data": {
        "updatedTime": 1703485237953,
        "symbol": "BTCUSDT",
        "side": "Sell",
        "size": "0.003",
        "price": "43511.70"
    }
}
```

