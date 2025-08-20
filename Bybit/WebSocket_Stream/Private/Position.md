# Position

Subscribe to the position stream to see changes to your position data in real-time.

**All-In-One Topic:** position  
**Categorised Topic:** position.linear, position.inverse, position.option

> **info**  
> All-In-One topic and Categorised topic cannot be in the same subscription request  
> All-In-One topic: Allow you to listen to all categories (linear, inverse, option) websocket updates  
> Categorised Topic: Allow you to listen only to specific category websocket updates

> **tip**  
> Every time when you create/amend/cancel an order, the position topic will generate a new message (regardless if there's any actual change)

### Response Parameters

| Parameter          | Type    | Comments                                                                                                                                                                             |
|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                 | string  | Message ID                                                                                                                                                                           |
| topic              | string  | Topic name                                                                                                                                                                          |
| creationTime       | number  | Data created timestamp (ms)                                                                                                                                                          |
| data               | array   | Object                                                                                                                                                                               |

Inside `data` array objects:

| Parameter          | Type    | Comments                                                                                                                                                                             |
|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| category           | string  | Product type                                                                                                                                                                         |
| symbol             | string  | Symbol name                                                                                                                                                                          |
| side               | string  | Position side. Buy: long, Sell: short <br>one-way mode: classic & UTA1.0(inverse), an empty position returns None. <br>UTA2.0(linear, inverse) & UTA1.0(linear): either one-way or hedge mode returns empty string `""` for an empty position.                                  |
| size               | string  | Position size                                                                                                                                                                        |
| positionIdx        | integer | Used to identify positions in different position modes                                                                                                                              |
| tradeMode          | integer | Trade mode  <br>Classic & UTA1.0(inverse): 0: cross-margin, 1: isolated margin  <br>UTA2.0, UTA1.0(except inverse): deprecated, always 0, check Get Account Info to know the margin mode                                   |
| positionValue      | string  | Position value                                                                                                                                                                       |
| riskId             | integer | Risk tier ID  <br>for portfolio margin mode, this field returns 0, which means risk limit rules are invalid                                                                                              |
| riskLimitValue     | string  | Risk limit value  <br>for portfolio margin mode, this field returns 0, which means risk limit rules are invalid                                                                                              |
| entryPrice         | string  | Entry price                                                                                                                                                                         |
| markPrice          | string  | Mark price                                                                                                                                                                          |
| leverage           | string  | Position leverage  <br>for portfolio margin mode, this field returns `""`, which means leverage rules are invalid                                                                                              |
| positionBalance    | string  | Position margin  <br>Classic & UTA1.0(inverse) can refer to this field to get the position initial margin                                                                                                    |
| autoAddMargin      | integer | Whether to add margin automatically. 0: false, 1: true. For UTA, meaningful only when UTA enables ISOLATED_MARGIN                                                                      |
| positionIM         | string  | Initial margin  <br>Classic & UTA1.0(inverse): ignore this field  <br>UTA portfolio margin mode, it returns `""`                                                                                                                                                |
| positionIMByMp     | string  | Initial margin calculated by mark price  <br>Classic & UTA1.0(inverse): ignore this field  <br>UTA portfolio margin mode, it returns `""`                                                                                                                                                |
| positionMM         | string  | Maintenance margin  <br>Classic & UTA1.0(inverse): ignore this field  <br>UTA portfolio margin mode, it returns `""`                                                                                                                                              |
| positionMMByMp     | string  | Maintenance margin calculated by mark price  <br>Classic & UTA1.0(inverse): ignore this field  <br>UTA portfolio margin mode, it returns `""`                                                                                                                                              |
| liqPrice           | string  | Position liquidation price  <br>UTA1.0(inverse), UTA(isolated margin enabled) & Classic account: real price for isolated and cross positions, keeps `""` when `liqPrice <= minPrice` or `liqPrice >= maxPrice`  <br>UTA (Cross margin mode): estimated price for cross positions (unified mode controls risk rate), keeps `""` if outside limits  <br>Empty for Portfolio Margin Mode — no liquidation price provided                            |
| bustPrice          | string  | Bankruptcy price  <br>Unified mode returns `""`, no position bankruptcy price (except UTA1.0(inverse))                                                                                                          |
| tpslMode           | string  | Deprecated, meaningless here, always "Full"                                                                                                                                          |
| takeProfit         | string  | Take profit price                                                                                                                                                                    |
| stopLoss           | string  | Stop loss price                                                                                                                                                                      |
| trailingStop       | string  | Trailing stop                                                                                                                                                                        |
| unrealisedPnl      | string  | Unrealised profit and loss                                                                                                                                                           |
| curRealisedPnl     | string  | The realised PnL for the current holding position                                                                                                                                    |
| sessionAvgPrice    | string  | USDC contract session avg price, same as avg entry price shown in the web UI                                                                                                         |
| delta              | string  | Delta. Only pushed when subscribing to option position                                                                                                                               |
| gamma              | string  | Gamma. Only pushed when subscribing to option position                                                                                                                               |
| vega               | string  | Vega. Only pushed when subscribing to option position                                                                                                                                |
| theta              | string  | Theta. Only pushed when subscribing to option position                                                                                                                               |
| cumRealisedPnl     | string  | Cumulative realised PnL  <br>Futures & Perp: all-time cumulative realised P&L  <br>Option: realised P&L when holding that position                                                                               |
| positionStatus     | string  | Position status: Normal, Liq, Adl                                                                                                                                                    |
| adlRankIndicator   | integer | Auto-deleverage rank indicator (Auto-Deleveraging)                                                                                                                                   |
| isReduceOnly       | boolean | Useful when Bybit lowers the risk limit  <br>true: Only allowed to reduce the position — consider measures to remove reduceOnly mark afterwards  <br>false: No restriction, position is under risk when risk limit adjusted systematically  <br>Only meaningful for isolated margin & cross margin of USDT Perp, USDC Perp, USDC Futures, Inverse Perp and Inverse Futures; meaningless for others |
| mmrSysUpdatedTime  | string  | Timestamp (ms) when MMR forcibly adjusted by system if isReduceOnly=true; else timestamp when MMR was adjusted by system  <br>Returns `""` if no system adjustment  <br>Meaningful for isolated and cross margin of USDT Perp, USDC Perp, USDC Futures, Inverse Perp and Inverse Futures only                                        |
| leverageSysUpdatedTime | string  | Timestamp when leverage forcibly adjusted by system if isReduceOnly=true; else timestamp of adjustment  <br>Returns `""` if none  <br>Meaningful for same accounts as mmrSysUpdatedTime                                                      |
| createdTime        | string  | Timestamp of first time a position was created on this symbol (ms)                                                                                                                   |
| updatedTime        | string  | Position data updated timestamp (ms)                                                                                                                                                 |
| seq                | long    | Cross sequence, used to associate each fill and each position update  <br>Different symbols may have same seq, use seq + symbol to identify uniquely  <br>Returns "-1" if symbol has never been traded  <br>Returns seq updated by last transaction when settings like leverage, risk limit change                                                                |

### Subscribe Example

```json
{
    "op": "subscribe",
    "args": [
        "position"
    ]
}
```

```python
from pybit.unified_trading import WebSocket
from time import sleep

ws = WebSocket(
    testnet=True,
    channel_type="private",
    api_key="xxxxxxxxxxxxxxxxxx",
    api_secret="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
)

def handle_message(message):
    print(message)

ws.position_stream(callback=handle_message)

while True:
    sleep(1)
```

### Stream Example

```json
{
    "id": "1003076014fb7eedb-c7e6-45d6-a8c1-270f0169171a",
    "topic": "position",
    "creationTime": 1697682317044,
    "data": [
        {
            "positionIdx": 2,
            "tradeMode": 0,
            "riskId": 1,
            "riskLimitValue": "2000000",
            "symbol": "BTCUSDT",
            "side": "",
            "size": "0",
            "entryPrice": "0",
            "leverage": "10",
            "positionValue": "0",
            "positionBalance": "0",
            "markPrice": "28184.5",
            "positionIM": "0",
            "positionIMByMp": "0",
            "positionMM": "0",
            "positionMMByMp": "0",
            "takeProfit": "0",
            "stopLoss": "0",
            "trailingStop": "0",
            "unrealisedPnl": "0",
            "curRealisedPnl": "1.26",
            "cumRealisedPnl": "-25.06579337",
            "sessionAvgPrice": "0",
            "createdTime": "1694402496913",
            "updatedTime": "1697682317038",
            "tpslMode": "Full",
            "liqPrice": "0",
            "bustPrice": "",
            "category": "linear",
            "positionStatus": "Normal",
            "adlRankIndicator": 0,
            "autoAddMargin": 0,
            "leverageSysUpdatedTime": "",
            "mmrSysUpdatedTime": "",
            "seq": 8327597863,
            "isReduceOnly": false
        }
    ]
}
```

***

