# Get Position info

Query real-time position data, such as position size, cumulative realized PNL, etc.

info



UTA2.0(inverse)

You can query all open positions with /v5/position/list?category=inverse;
Cannot query mul
tip
le symbols in one request
UTA1.0(inverse) & Classic (inverse)

You can query all open positions with /v5/position/list?category=inverse;

symbol parameter can pass up to 10 symbols, e.g., symbol=BTCUSD,ETHUSD


HTTP Request
```http
GET /v5/position/list
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type<br> UTA2.0, UTA1.0: linear, inverse, option<br> Classic account: linear, inverse |
| symbol | false | string | Symbol name, like BTCUSDT, uppercase only <br> If symbol passed, it returns data regardless of having position or not. <br> If symbol=null and settleCoin specified, it returns position size greater than zero. |
| baseCoin | false | string | Base coin, uppercase only. option only. Return all option positions if not passed |
| settleCoin | false | string | Settle coin<br> linear: either symbol or settleCoin is required. symbol has a higher priority |
| limit | false | integer | Limit for data size per page. [1, 200]. Default: 20 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter                | Type    | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| category                 | string  | Product type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| nextPageCursor           | string  | Refer to the cursor request parameter                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| list                     | array   | Object                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > positionIdx            | integer | Position idx, used to identify positions in different position modes<br>0: One-Way Mode<br>1: Buy side of both side mode<br>2: Sell side of both side mode                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > riskId                 | integer | Risk tier ID<br>for portfolio margin mode, this field returns 0, which means risk limit rules are invalid                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > riskLimitValue         | string  | Risk limit value<br>for portfolio margin mode, this field returns 0, which means risk limit rules are invalid                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > symbol                 | string  | Symbol name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > side                   | string  | Position side. Buy: long, Sell: short<br>one-way mode: classic & UTA1.0(inverse), an empty position returns None.<br>UTA2.0(linear, inverse) & UTA1.0(linear): either one-way or hedge mode returns an empty string "" for an empty position.                                                                                                                                                                                                                                                                                                                                                                                                          |
| > size                   | string  | Position size, always positive                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > avgPrice               | string  | Average entry price<br>For USDC Perp & Futures, it indicates average entry price, and it will not be changed with 8-hour session settlement                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > positionValue          | string  | Position value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > tradeMode              | integer | Trade mode<br>Classic & UTA1.0(inverse): 0: cross-margin, 1: isolated margin<br>UTA2.0, UTA1.0(execpt inverse): deprecated, always 0, check Get Account info
 to know the margin mode                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > autoAddMargin          | integer | Whether to add margin automatically when using isolated margin mode<br>0: false<br>1: true                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > positionStatus         | String  | Position status. Normal, Liq, Adl                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > leverage               | string  | Position leverage<br>for portfolio margin mode, this field returns "", which means leverage rules are invalid                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > markPrice              | string  | Mark price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > liqPrice               | string  | Position liquidation price<br>UTA2.0(isolated margin), UTA1.0(isolated margin), UTA1.0(inverse), Classic account:<br>it is the real price for isolated and cross positions, and keeps "" when liqPrice <= minPrice or liqPrice >= maxPrice<br>UTA2.0(Cross margin), UTA1.0(Cross margin):<br>it is an estimated price for cross positions(because the unified mode controls the risk rate according to the account), and keeps "" when liqPrice <= minPrice or liqPrice >= maxPricethis field is empty for Portfolio Margin Mode, and no liquidation price will be provided                                                                            |
| > bustPrice              | string  | Bankruptcy price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > positionIM             | string  | Initial margin<br>Classic & UTA1.0(inverse): ignore this field<br>UTA portfolio margin mode, it returns ""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > positionIMByMp         | string  | Initial margin calculated by mark price<br>Classic & UTA1.0(inverse) : ignore this field<br>UTA portfolio margin mode, it returns ""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > positionMM             | string  | Maintenance margin<br>Classic & UTA1.0(inverse): ignore this field<br>UTA portfolio margin mode, it returns ""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > positionMMByMp         | string  | Maintenance margin calculated by mark price<br>Classic & UTA1.0(inverse) : ignore this field<br>UTA portfolio margin mode, it returns ""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| > positionBalance        | string  | Position margin<br>Classic & UTA1.0(inverse) can refer to this field to get the position initial margin plus position closing fee                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > takeProfit             | string  | Take profit price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > stopLoss               | string  | Stop loss price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > trailingStop           | string  | Trailing stop (The distance from market price)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > sessionAvgPrice        | string  | USDC contract session avg price, it is the same figure as avg entry price shown in the web UI                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > delta                  | string  | Delta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > gamma                  | string  | Gamma                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > vega                   | string  | Vega                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > theta                  | string  | Theta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > unrealisedPnl          | string  | Unrealised PnL                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > curRealisedPnl         | string  | The realised PnL for the current holding position                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > cumRealisedPnl         | string  | Cumulative realised pnl<br>Futures & Perps: it is the all time cumulative realised P&L<br>Option: always "", meaningless                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| > adlRankIndicator       | integer | Auto-deleverage rank indicator. What is Auto-Deleveraging?                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > createdTime            | string  | Timestamp of the first time a position was created on this symbol (ms)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > updatedTime            | string  | Position updated timestamp (ms)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > seq                    | long    | Cross sequence, used to associate each fill and each position update<br>Different symbols may have the same seq, please use seq + symbol to check unique<br>Returns "-1" if the symbol has never been traded<br>Returns the seq updated by the last transaction when there are settings like leverage, risk limit                                                                                                                                                                                                                                                                                                                                      |
| > isReduceOnly           | boolean | Useful when Bybit lower the risk limit<br>true: Only allowed to reduce the position. You can consider a series of measures, e.g., lower the risk limit, decrease leverage or reduce the position, add margin, or cancel orders, after these operations, you can call confirm new risk limit endpoint to check if your position can be removed the reduceOnly mark<br>false: There is no restriction, and it means your position is under the risk when the risk limit is systematically adjusted<br>Only meaningful for isolated margin & cross margin of USDT Perp, USDC Perp, USDC Futures, Inverse Perp and Inverse Futures, meaningless for others |
| > mmrSysUpdatedTime      | string  | Useful when Bybit lower the risk limit<br>When isReduceOnly=true: the timestamp (ms) when the MMR will be forcibly adjusted by the systemWhen isReduceOnly=false: the timestamp when the MMR had been adjusted by system<br>It returns the timestamp when the system operates, and if you manually operate, there is no timestamp<br>Keeps "" by default, if there was a lower risk limit system adjustment previously, it shows that system operation timestamp<br>Only meaningful for isolated margin & cross margin of USDT Perp, USDC Perp, USDC Futures, Inverse Perp and Inverse Futures, meaningless for others                                 |
| > leverageSysUpdatedTime | string  | Useful when Bybit lower the risk limit<br>When isReduceOnly=true: the timestamp (ms) when the leverage will be forcibly adjusted by the systemWhen isReduceOnly=false: the timestamp when the leverage had been adjusted by system<br>It returns the timestamp when the system operates, and if you manually operate, there is no timestamp<br>Keeps "" by default, if there was a lower risk limit system adjustment previously, it shows that system operation timestamp<br>Only meaningful for isolated margin & cross margin of USDT Perp, USDC Perp, USDC Futures, Inverse Perp and Inverse Futures, meaningless for others                       |
| > tpslMode               | string  | deprecated, always "Full"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
---


Request Example

HTTP
 
  
  
```http
GET /v5/position/list?category=inverse&symbol=BTCUSD HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672280218882
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "positionIdx": 0,
                "riskId": 1,
                "riskLimitValue": "150",
                "symbol": "BTCUSD",
                "side": "Sell",
                "size": "300",
                "avgPrice": "27464.50441675",
                "positionValue": "0.01092319",
                "tradeMode": 0,
                "positionStatus": "Normal",
                "autoAddMargin": 1,
                "adlRankIndicator": 2,
                "leverage": "10",
                "positionBalance": "0.00139186",
                "markPrice": "28224.50",
                "liqPrice": "",
                "bustPrice": "999999.00",
                "positionMM": "0.0000015",
                "positionMMByMp": "0.0000015",
                "positionIM": "0.00010923",
                "positionIMByMp": "0.00010923",
                "tpslMode": "Full",
                "takeProfit": "0.00",
                "stopLoss": "0.00",
                "trailingStop": "0.00",
                "unrealisedPnl": "-0.00029413",
                "curRealisedPnl": "0.00013123",
                "cumRealisedPnl": "-0.00096902",
                "seq": 5723621632,
                "isReduceOnly": false,
                "mmrSysUpdateTime": "",
                "leverageSysUpdatedTime": "",
                "sessionAvgPrice": "",
                "createdTime": "1676538056258",
                "updatedTime": "1697673600012"
            }
        ],
        "nextPageCursor": "",
        "category": "inverse"
    },
    "retExtinfo
": {},
    "time": 1697684980172
}
```

# Set Leverage
info

According to the risk limit, leverage affects the maximum position value that can be opened, that is, the greater the leverage, the smaller the maximum position value that can be opened, and vice versa. Learn more


HTTP Request
```http
POST /v5/position/set-leverage
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type |
| UTA2.0, UTA1.0: linear, inverse |
| Classic account: linear, inverse |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| buyLeverage | true | string | [1, max leverage] |
| one-way mode: buyLeverage must be the same as sellLeverage |
| Hedge mode: |
| Classic account & UTA (isolated margin): buyLeverage and sellLeverage can be different; |
| UTA (cross margin): buyLeverage must be the same as sellLeverage |
| sellLeverage | true | string | [1, max leverage] |

---



Response Parameters
None


Request Example

HTTP
 
  
  
```http
POST /v5/position/set-leverage HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672281605082
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "category": "linear",
    "symbol": "BTCUSDT",
    "buyLeverage": "6",
    "sellLeverage": "6"
```

}

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtinfo
": {},
    "time": 1672281607343
}
```

