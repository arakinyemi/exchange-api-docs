# Get Order Price Limit
For derivative trading order price limit, refer to announcement
For spot trading order price limit, refer to announcement


HTTP Request
```http
GET /v5/market/price-limit
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | false | string | Product type. spot,linear,inverse |
| When category is not passed, use linear by default |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| symbol | string | Symbol name |
| buyLmt | string | Highest Bid Price |
| sellLmt | string | Lowest Ask Price |
| ts | string | timestamp in milliseconds |

---

Request Example

HTTP
 
  
  
  
```http
GET /v5/market/price-limit?category=spot&symbol=BTCUSDT HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "symbol": "BTCUSDT",
        "buyLmt": "105878.10",
        "sellLmt": "103781.60",
        "ts": "1750302284491"
    },
    "retExtinfo
": {},
    "time": 1750302285376
}
```

