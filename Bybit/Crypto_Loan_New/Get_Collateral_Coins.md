# Get Collateral Coins
info

Does not need authentication.


HTTP Request
```http
GET /v5/crypto-loan-common/collateral-data
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| currency | false | string | Coin name, uppercase only |

---


Response Parameters
| Parameter                 | Type    | Comments                                                    |
|---------------------------|---------|-------------------------------------------------------------|
| collateralRatioConfigList | array   | Object                                                      |
| > collateralRatioList     | array   | Object                                                      |
| >> collateralRatio        | string  | Collateral ratio                                            |
| >> maxValue               | string  | Max qty                                                     |
| >> minValue               | string  | Min qty                                                     |
| > currencies              | string  | Currenies with the same collateral ratio, e.g., BTC,ETH,XRP |
| currencyLiquidationList   | array   | Object                                                      |
| > currency                | string  | Coin name                                                   |
| > liquidationOrder        | integer | Liquidation order                                           |

---

Request Example

HTTP
 
  
```http
GET /v5/crypto-loan/collateral-data?currency=ETH&vipLevel=PRO1 HTTP/1.1
Host: api.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "vipCoinList": [
            {
                "list": [
                    {
                        "collateralAccuracy": 8,
                        "currency": "ETH",
                        "initialLTV": "0.8",
                        "liquidationLTV": "0.95",
                        "marginCallLTV": "0.87",
                        "maxLimit": "32000"
                    }
                ],
                "vipLevel": "PRO1"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1728618590498
}
```

