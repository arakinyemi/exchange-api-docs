# Get Tiered Collateral Ratio
UTA loan tiered collateral ratio

info

Does not need authentication.


HTTP Request
```http
GET /v5/spot-margin-trade/collateral
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| currency | false | string | Coin name, uppercase only |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > currency | string | Coin name |
| > collateralRatioList | array | Object |
| >> maxQty | string | Upper limit(in coin) of the tiered range, "" means positive infinity |
| >> minQty | string | lower limit(in coin) of the tiered range |
| >> collateralRatio | string | Collateral ratio |

---

Request Example

HTTP
 
  
```http
GET /v5/spot-margin-trade/collateral?currency=BTC HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "currency": "BTC",
                "collateralRatioList": [
                    {
                        "minQty": "0",
                        "maxQty": "1000000",
                        "collateralRatio": "0.85"
                    },
                    {
                        "minQty": "1000000",
                        "maxQty": "",
                        "collateralRatio": "0"
                    }
                ]
            }
        ]
    },
    "retExtinfo
": "{}",
    "time": 1739848984945
}
```

