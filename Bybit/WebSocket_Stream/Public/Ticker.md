# Ticker

Subscribe to the ticker stream.

> **Note:** This topic utilises the snapshot field and delta field. If a response param is not found in the message, then its value has not changed.
> - Spot & Option tickers message are snapshot only
> - **Push frequency:** Derivatives & Options - 100ms, Spot - real-time

### Topic:
`tickers.{symbol}`

### Response Parameters

| Parameter | Type | Comments |
|-----------|------|----------|
| topic | string | Topic name |
| ts | number | The timestamp (ms) that the system generates the data |
| type | string | Data type. snapshot |
| cs | integer | Cross sequence |
| data | array | Object |
| > symbol | string | Symbol name |
| > lastPrice | string | Last price |
| > highPrice24h | string | The highest price in the last 24 hours |
| > lowPrice24h | string | The lowest price in the last 24 hours |
| > prevPrice24h | string | Percentage change of market price relative to 24h |
| > volume24h | string | Volume for 24h |
| > turnover24h | string | Turnover for 24h |
| > price24hPcnt | string | Percentage change of market price relative to 24h |
| > usdIndexPrice | string | USD index price used to calculate USD value of the assets in Unified account. non-collateral margin coin returns "" |

### Subscribe Example

```python
from pybit.unified_trading import WebSocket
from time import sleep
ws = WebSocket(
    testnet=True,
    channel_type="spot",
)
def handle_message(message):
    print(message)
ws.ticker_stream(
    symbol="BTCUSDT",
    callback=handle_message
)
while True:
    sleep(1)
```

### Response Example

```json
{
    "topic": "tickers.BTCUSDT",
    "ts": 1673853746003,
    "type": "snapshot",
    "cs": 2588407389,
    "data": {
        "symbol": "BTCUSDT",
        "lastPrice": "21109.77",
        "highPrice24h": "21426.99",
        "lowPrice24h": "20575",
        "prevPrice24h": "20704.93",
        "volume24h": "6780.866843",
        "turnover24h": "141946527.22907118",
        "price24hPcnt": "0.0196",
        "usdIndexPrice": "21120.2400136"
    }
}
```
