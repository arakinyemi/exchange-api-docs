### SubType Codes (Transaction Types)

| Code | Meaning                                |
|-------|--------------------------------------|
| 1     | Buy                                  |
| 2     | Sell                                 |
| 3     | Open long                           |
| 4     | Open short                          |
| 5     | Close long                         |
| 6     | Close short                        |
| 100   | Partial liquidation close long      |
| 101   | Partial liquidation close short     |
| 102   | Partial liquidation buy             |
| 103   | Partial liquidation sell            |
| 104   | Liquidation long                   |
| 105   | Liquidation short                  |
| 106   | Liquidation buy                   |
| 107   | Liquidation sell                  |
| 110   | Liquidation transfer in            |
| 111   | Liquidation transfer out           |
| 118   | System token conversion transfer in|
| 119   | System token conversion transfer out|
| 112   | Delivery long                     |
| 113   | Delivery short                    |
| 125   | ADL close long                   |
| 126   | ADL close short                  |
| 127   | ADL buy                        |
| 128   | ADL sell                       |
| 212   | Auto borrow of quick margin       |
| 213   | Auto repay of quick margin        |
| 204   | Block trade buy                  |
| 205   | Block trade sell                 |
| 206   | Block trade open long            |
| 207   | Block trade open short           |
| 208   | Block trade close long           |
| 209   | Block trade close short          |
| 236   | Easy convert in                 |
| 237   | Easy convert out                |
| 270   | Spread trading buy              |
| 271   | Spread trading sell             |
| 272   | Spread trading open long        |
| 273   | Spread trading open short       |
| 274   | Spread trading close long       |
| 275   | Spread trading close short      |
| 324   | Move position buy               |
| 325   | Move position sell              |
| 326   | Move position open long         |
| 327   | Move position open short        |
| 328   | Move position close long        |
| 329   | Move position close short       |
| 376   | Collateralized borrowing auto conversion buy  |
| 377   | Collateralized borrowing auto conversion sell |

Response Example
```json
{
    "code": "0",
    "data": [
        {
            "side": "buy",
            "fillSz": "0.00192834",
            "fillPx": "51858",
            "fillPxVol": "",
            "fillFwdPx": "",
            "fee": "-0.00000192834",
            "fillPnl": "0",
            "ordId": "680800019749904384",
            "feeRate": "-0.001",
            "instType": "SPOT",
            "fillPxUsd": "",
            "instId": "BTC-USDT",
            "clOrdId": "",
            "posSide": "net",
            "billId": "680800019754098688",
            "subType": "1",
            "fillMarkVol": "",
            "tag": "",
            "fillTime": "1708587373361",
            "execType": "T",
            "fillIdxPx": "",
            "tradeId": "744876980",
            "fillMarkPx": "",
            "feeCcy": "BTC",
            "ts": "1708587373362"
        }
    ],
    "msg": ""
}
```

Response Parameters

| Parameter       | Type   | Description                                                    |
|-----------------|--------|----------------------------------------------------------------|
| instType        | String | Instrument type                                               |
| instId          | String | Instrument ID                                                 |
| tradeId         | String | Last trade ID                                                |
| ordId           | String | Order ID ("" for block trading)                             |
| clOrdId         | String | Client-supplied order ID ("" for block trading)             |
| billId          | String | Bill ID                                                      |
| subType         | String | Transaction type code                                        |
| tag             | String | Order tag                                                   |
| fillPx          | String | Last filled price (same as px from "Get bills details")     |
| fillSz          | String | Last filled quantity                                        |
| fillIdxPx       | String | Index price at trade execution time                        |
| fillPnl         | String | Last filled PnL (applicable to closing orders)             |
| fillPxVol       | String | Implied volatility when filled (Only for options)          |
| fillPxUsd       | String | Options price when filled (Only for options)                |
| fillMarkVol     | String | Mark volatility when filled (Only for options)              |
| fillFwdPx       | String | Forward price when filled (Only for options)                |
| fillMarkPx      | String | Mark price when filled (Applicable to FUTURES, SWAP, OPTION) |
| side            | String | Order side: buy/sell                                        |
| posSide         | String | Position side: long/short/net                              |
| execType        | String | Liquidity taker or maker (T: taker, M: maker)               |
| feeCcy          | String | Trading fee or rebate currency                              |
| fee             | String | Amount of fee or rebate                                    |
| ts              | String | Data generation time (Unix timestamp ms)                   |
| fillTime        | String | Trade time (same as fillTime for order channel)             |
| feeRate         | String | Fee rate (for SPOT and MARGIN only)                        |
