# Get Tickers
Query for the latest price snapshot, best bid/ask price, and trading volume in the last 24 hours.

Covers: Spot / USDT contract / USDC contract / Inverse contract / Option

info

If category=option, symbol or baseCoin must be passed.


HTTP Request
```http
GET /v5/market/tickers
```


Request Parameters
| Parameter | Required | Type   |                     Comments                     |
|-----------|----------|--------|:------------------------------------------------:|
| category  | true     | string | Product type. spot,linear,inverse,option         |
| symbol    | false    | string | Symbol name, like BTCUSDT, uppercase only        |
| baseCoin  | false    | string | Base coin, uppercase only. Apply to option only  |
| expDate   | false    | string | Expiry date. e.g., 25DEC22. Apply to option only |


Response Parameters
|    Parameter    |  Type  |                                                                                              Comments                                                                                             |
|---------------|------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| category        | string | Product type                                                                                                                                                                                      |
| list            | array  | Object                                                                                                                                                                                            |
| > symbol        | string | Symbol name                                                                                                                                                                                       |
| > bid1Price     | string | Best bid price                                                                                                                                                                                    |
| > bid1Size      | string | Best bid size                                                                                                                                                                                     |
| > ask1Price     | string | Best ask price                                                                                                                                                                                    |
| > ask1Size      | string | Best ask size                                                                                                                                                                                     |
| > lastPrice     | string | Last price                                                                                                                                                                                        |
| > prevPrice24h  | string | Market price 24 hours ago                                                                                                                                                                         |
| > price24hPcnt  | string | Percentage change of market price relative to 24h                                                                                                                                                 |
| > highPrice24h  | string | The highest price in the last 24 hours                                                                                                                                                            |
| > lowPrice24h   | string | The lowest price in the last 24 hours                                                                                                                                                             |
| > turnover24h   | string | Turnover for 24h                                                                                                                                                                                  |
| > volume24h     | string | Volume for 24h                                                                                                                                                                                    |
| > usdIndexPrice | string | USD index price used to calculate USD value of the assets in Unified account<br>non-collateral margin coin returns ""<br>Only those trading pairs like "XXX/USDT" or "XXX/USDC" have the value |

---

Request Example
```http
GET /v5/market/tickers?category=spot&symbol=BTCUSDT HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "category": "spot",
        "list": [
            {
                "symbol": "BTCUSDT",
                "bid1Price": "20517.96",
                "bid1Size": "2",
                "ask1Price": "20527.77",
                "ask1Size": "1.862172",
                "lastPrice": "20533.13",
                "prevPrice24h": "20393.48",
                "price24hPcnt": "0.0068",
                "highPrice24h": "21128.12",
                "lowPrice24h": "20318.89",
                "turnover24h": "243765620.65899866",
                "volume24h": "11801.27771",
                "usdIndexPrice": "20784.12009279"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1673859087947
}
```

