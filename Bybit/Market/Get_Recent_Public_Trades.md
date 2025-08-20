# Get Recent Public Trades
Query recent public spread trading history in Bybit.


HTTP Request
```http
GET /v5/market/recent-trade
```

Request Parameters
| Parameter  | Required | Type    |                                                Comments                                               |
|------------|----------|---------|-----------------------------------------------------------------------------------------------------|
| category   | true     | string  | Product type. spot,linear,inverse,option                                                              |
| symbol     | false    | string  | Symbol name, like BTCUSDT, uppercase only required for spot/linear/inverse optional for option        |
| baseCoin   | false    | string  | Base coin, uppercase only Apply to option only If the field is not passed, return BTC data by default |
| optionType | false    | string  | Option type. Call or Put. Apply to option only                                                        |
| limit      | false    | integer | Limit for data size per page spot: [1,60], default: 60 others: [1,1000], default: 500                 |

Response Parameters
| Parameter      | Type    | Comments                             |
|----------------|---------|--------------------------------------|
| category       | string  | Products category                    |
| list           | array   | Object                               |
| > execId       | string  | Execution ID                         |
| > symbol       | string  | Symbol name                          |
| > price        | string  | Trade price                          |
| > size         | string  | Trade size                           |
| > side         | string  | Side of taker Buy, Sell              |
| > time         | string  | Trade time (ms)                      |
| > isBlockTrade | boolean | Whether the trade is block trade     |
| > isRPITrade   | boolean | Whether the trade is RPI trade       |
| > mP           | string  | Mark price, unique field for option  |
| > iP           | string  | Index price, unique field for option |
| > mIv          | string  | Mark iv, unique field for option     |
| > iv           | string  | iv, unique field for option          |
| > seq          | string  | cross sequence                       |

---

Request Example
```http
GET /v5/market/recent-trade?category=spot&symbol=BTCUSDT&limit=1 HTTP/1.1
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
                "execId": "2100000000007764263",
                "symbol": "BTCUSDT",
                "price": "16618.49",
                "size": "0.00012",
                "side": "Buy",
                "time": "1672052955758",
                "isBlockTrade": false,
                "isRPITrade": true,
                "seq":"123456"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1672053054358
}
```



