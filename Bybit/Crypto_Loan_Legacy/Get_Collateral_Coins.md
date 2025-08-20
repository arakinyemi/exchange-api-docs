# Get Collateral Coins
info

Does not need authentication.


HTTP Request
```http
GET /v5/crypto-loan/collateral-data
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| vipLevel | false | string | Vip level<br> VIP0, VIP1, VIP2, VIP3, VIP4, VIP5, VIP99(supreme VIP)<br> PRO1, PRO2, PRO3, PRO4, PRO5, PRO6 |
| currency | false | string | Coin name, uppercase only |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| vipCoinList | array | Object |
| > list | array | Object |
| >> collateralAccuracy | integer | Valid collateral coin precision |
| >> initialLTV | string | The Initial LTV ratio determines the initial amount of coins that can be borrowed. The initial LTV ratio may vary for different collateral |
| >> marginCallLTV | string | If the LTV ratio (Loan Amount/Collateral Amount) reaches the threshold, you will be required to add more collateral to your loan |
| >> liquidationLTV | string | If the LTV ratio (Loan Amount/Collateral Amount) reaches the threshold, Bybit will liquidate your collateral assets to repay your loan and interest in full |
| >> maxLimit | string | Collateral limit |
| > vipLevel | string | Vip level |

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

