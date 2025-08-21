### Get / Transaction details (last 1 year)

This endpoint can retrieve data from the last 1 year since July 1, 2024.

Rate Limit: 10 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

HTTP Request  
`GET /api/v5/trade/fills-history`

Request Example  

`GET /api/v5/trade/fills-history?instType=SPOT`

Request Parameters

| Parameter  | Type   | Required | Description                                      |
|------------|--------|----------|--------------------------------------------------|
| instType   | String | YES      | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION |
| uly        | String | No       | Underlying (Applicable to FUTURES/SWAP/OPTION)   |
| instFamily | String | No       | Instrument family (Applicable to FUTURES/SWAP/OPTION) |
| instId     | String | No       | Instrument ID, e.g. BTC-USDT                     |
| ordId      | String | No       | Order ID                                         |
| subType    | String | No       | Transaction type (see list below)                |
| after      | String | No       | Pagination for records earlier than requested billId |
| before     | String | No       | Pagination for records newer than requested billId   |
| begin      | String | No       | Filter with a begin timestamp (ms)               |
| end        | String | No       | Filter with an end timestamp (ms)                |
| limit      | String | No       | Number of results. Max 100. Default 100          |

#### subType Transaction Types

| Code | Description                               |
|------|-------------------------------------------|
| 1    | Buy                                      |
| 2    | Sell                                     |
| 3    | Open long                                |
| 4    | Open short                               |
| 5    | Close long                               |
| 6    | Close short                              |
| 100  | Partial liquidation close long           |
| 101  | Partial liquidation close short          |
| 102  | Partial liquidation buy                  |
| 103  | Partial liquidation sell                 |
| 104  | Liquidation long                         |
| 105  | Liquidation short                        |
| 106  | Liquidation buy                          |
| 107  | Liquidation sell                         |
| 110  | Liquidation transfer in                  |
| 111  | Liquidation transfer out                 |
| 118  | System token conversion transfer in      |
| 119  | System token conversion transfer out     |
| 112  | Delivery long                            |
| 113  | Delivery short                           |
| 125  | ADL close long                           |
| 126  | ADL close short                          |
| 127  | ADL buy                                  |
| 128  | ADL sell                                 |
| 212  | Auto borrow of quick margin              |
| 213  | Auto repay of quick margin               |
| 204  | Block trade buy                          |
| 205  | Block trade sell                         |
| 206  | Block trade open long                    |
| 207  | Block trade open short                   |
| 208  | Block trade close long                   |
| 209  | Block trade close short                  |
| 236  | Easy convert in                          |
| 237  | Easy convert out                         |
| 270  | Spread trading buy                       |
| 271  | Spread trading sell                      |
| 272  | Spread trading open long                 |
| 273  | Spread trading open short                |
| 274  | Spread trading close long                |
| 275  | Spread trading close short               |
| 324  | Move position buy                        |
| 325  | Move position sell                       |
| 326  | Move position open long                  |
| 327  | Move position open short                 |
| 328  | Move position close long                 |
| 329  | Move position close short                |
| 376  | Collateralized borrowing auto conversion buy |
| 377  | Collateralized borrowing auto conversion sell |

Response Example  

```
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

| Parameter    | Type   | Description     |
|--------------|--------|-----------------|
| instType     | String | Instrument type |
| instId       | String | Instrument ID   |
| tradeId      | String | Last trade ID   |
| ordId        | String | Order ID        |
| clOrdId      | String | Client Order ID as assigned by the client |
| billId       | String | Bill ID         |
| subType      | String | Transaction type|
| tag          | String | Order tag       |
| fillPx       | String | Last filled price|
| fillSz       | String | Last filled quantity|
| fillIdxPx    | String | Index price at the moment of trade execution|
| fillPnl      | String | Last filled profit and loss|
| fillPxVol    | String | Implied volatility when filled|
| fillPxUsd    | String | Options price when filled, in the unit of USD|
| fillMarkVol  | String | Mark volatility when filled|
| fillFwdPx    | String | Forward price when filled|
| fillMarkPx   | String | Mark price when filled|
| side         | String | Order side      |
| posSide      | String | Position side   |
| execType     | String | Liquidity taker or maker|
| feeCcy       | String | Trading fee or rebate currency|
| fee          | String | The amount of trading fee or rebate|
| ts           | String | Data generation time (milliseconds)|
| fillTime     | String | Trade time      |
| feeRate      | String | Fee rate (SPOT/MARGIN only)|
