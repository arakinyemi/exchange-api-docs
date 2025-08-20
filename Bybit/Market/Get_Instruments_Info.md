# Get Instruments info

Query for the instrument specification of spread combinations.


HTTP Request
```http
GET /v5/market/instruments-info

```

Request Parameters
| category | true  | string  | Product type. spot,linear,inverse,option                                                                                                                    |
|----------|-------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| symbol   | false | string  | Symbol name, like BTCUSDT, uppercase only                                                                                                                   |
| status   | false | string  | Symbol status filter By default returns only Trading symbols Spot has Trading only linear & inverse: when status=PreLaunch, it returns Pre-Market contracts |
| baseCoin | false | string  | Base coin, uppercase only Applies to linear,inverse,option only option: returns BTC by default                                                              |
| limit    | false | integer | Limit for data size per page. [1, 1000]. Default: 500                                                                                                       |
| cursor   | false | string  | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set                                                          |


Response Parameters 
|      Parameter      |  Type  |                                                                                                                               Comments                                                                                                                               |   |
|-------------------|------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
| category            | string | Product type                                                                                                                                                                                                                                                         |   |
| list                | array  | Object                                                                                                                                                                                                                                                               |   |
| > symbol            | string | Symbol name                                                                                                                                                                                                                                                          |   |
| > baseCoin          | string | Base coin                                                                                                                                                                                                                                                            |   |
| > quoteCoin         | string | Quote coin                                                                                                                                                                                                                                                           |   |
| > innovation        | string | Whether or not this is an innovation zone token. 0: false, 1: true                                                                                                                                                                                                   |   |
| > status            | string | Instrument status                                                                                                                                                                                                                                                    |   |
| > marginTrading     | string | Margin trade symbol or not. This is to identify if the symbol support margin trading under different account modes. You may find some symbols not supporting margin buy or margin sell, so you need to go to Collateral info
 (UTA) to check if that coin is borrowable |   |
| > stTag             | string | Whether or not it has an special treatment label. 0: false, 1: true                                                                                                                                                                                                  |   |
| > lotSizeFilter     | Object | Size attributes                                                                                                                                                                                                                                                      |   |
| >> basePrecision    | string | The precision of base coin                                                                                                                                                                                                                                           |   |
| >> quotePrecision   | string | The precision of quote coin                                                                                                                                                                                                                                          |   |
| >> minOrderQty      | string | Minimum order quantity                                                                                                                                                                                                                                               |   |
| >> maxOrderQty      | string | Maximum order quantity                                                                                                                                                                                                                                               |   |
| >> minOrderAmt      | string | Minimum order amount                                                                                                                                                                                                                                                 |   |
| >> maxOrderAmt      | string | Maximum order amount                                                                                                                                                                                                                                                 |   |
| > priceFilter       | Object | Price attributes                                                                                                                                                                                                                                                     |   |
| >> tickSize         | string | The step to increase/reduce order price                                                                                                                                                                                                                              |   |
| > riskParameters    | Object | Risk parameters for limit order price, refer to announcement                                                                                                                                                                                                         |   |
| >> priceLimitRatioX | string | Ratio X                                                                                                                                                                                                                                                              |   |
| >> priceLimitRatioY | string | Ratio Y                                                                                                                                                                                                                                                              |   |

---

Request Example
```http
GET /v5/market/instruments-info
?category=spot&symbol=BTCUSDT HTTP/1.1
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
                "baseCoin": "BTC",
                "quoteCoin": "USDT",
                "innovation": "0",
                "status": "Trading",
                "marginTrading": "utaOnly",
                "stTag": "0",
                "lotSizeFilter": {
                    "basePrecision": "0.000001",
                    "quotePrecision": "0.00000001",
                    "minOrderQty": "0.000048",
                    "maxOrderQty": "71.73956243",
                    "minOrderAmt": "1",
                    "maxOrderAmt": "2000000"
                },
                "priceFilter": {
                    "tickSize": "0.01"
                }
                "riskParameters": {
                    "priceLimitRatioX": "0.01",
                    "priceLimitRatioY": "0.02"
                }
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1672712468011
}
```

