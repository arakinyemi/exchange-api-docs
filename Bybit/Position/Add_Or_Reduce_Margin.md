# Add Or Reduce Margin
Manually add or reduce margin for isolated margin position


HTTP Request
```http
POST /v5/position/add-margin
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type |
| UTA2.0, UTA1.0: linear, inverse |
| Classic account: linear, inverse |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| margin | true | string | Add or reduce. To add, then 10; To reduce, then -10. Support up to 4 decimal |
| positionIdx | false | integer | Used to identify positions in different position modes. For hedge mode position, this param is required <br> 0: one-way mode <br> 1: hedge-mode Buy side <br> 2: hedge-mode Sell side |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type |
| symbol | string | Symbol name |
| positionIdx | integer | Position idx, used to identify positions in different position modes <br> 0: One-Way Mode <br> 1: Buy side of both side mode <br> 2: Sell side of both side mode |
| riskId | integer | Risk limit ID |
| riskLimitValue | string | Risk limit value |
| size | string | Position size |
| avgPrice | string | Average entry price |
| liqPrice | string | Liquidation price |
| bustPrice | string | Bankruptcy price |
| markPrice | string | Last mark price |
| positionValue | string | Position value |
| leverage | string | Position leverage |
| autoAddMargin | integer | Whether to add margin automatically. 0: false, 1: true |
| positionStatus | String | Position status. Normal, Liq, Adl |
| positionIM | string | Initial margin |
| positionMM | string | Maintenance margin |
| takeProfit | string | Take profit price |
| stopLoss | string | Stop loss price |
| trailingStop | string | Trailing stop (The distance from market price) |
| unrealisedPnl | string | Unrealised PnL |
| cumRealisedPnl | string | Cumulative realised pnl |
| createdTime | string | Timestamp of the first time a position was created on this symbol (ms) |
| updatedTime | string | Position updated timestamp (ms) |

---


Request Example

HTTP
 
  
  
```http
POST /v5/position/add-margin HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1684234363665
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 97
```

```json
{
    "category": "inverse",
    "symbol": "ETHUSD",
    "margin": "0.01",
    "positionIdx": 0
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "category": "inverse",
        "symbol": "ETHUSD",
        "positionIdx": 0,
        "riskId": 11,
        "riskLimitValue": "500",
        "size": "200",
        "positionValue": "0.11033265",
        "avgPrice": "1812.70004844",
        "liqPrice": "1550.80",
        "bustPrice": "1544.20",
        "markPrice": "1812.90",
        "leverage": "12",
        "autoAddMargin": 0,
        "positionStatus": "Normal",
        "positionIM": "0.01926611",
        "positionMM": "0",
        "unrealisedPnl": "0.00001217",
        "cumRealisedPnl": "-0.04618929",
        "stopLoss": "0.00",
        "takeProfit": "0.00",
        "trailingStop": "0.00",
        "createdTime": "1672737740039",
        "updatedTime": "1684234363788"
    },
    "retExtinfo
": {},
    "time": 1684234363789
}
```

