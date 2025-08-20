# Get Risk Limit
Query for the risk limit.

Covers: USDT contract / USDC contract / Inverse contract

info

category=linear returns a data set of 15 symbols in each response. Please use the cursor param to get the next data set.


HTTP Request
```http
GET /v5/market/risk-limit
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type. linear,inverse |
| symbol | false | string | Symbol name, like BTCUSDT, uppercase only |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the data set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type |
| list | array | Object |
| > id | integer | Risk ID |
| > symbol | string | Symbol name |
| > riskLimitValue | string | Position limit |
| > maintenanceMargin | number | Maintain margin rate |
| > initialMargin | number | Initial margin rate |
| > isLowestRisk | integer | 1: true, 0: false |
| > maxLeverage | string | Allowed max leverage |
| > mmDeduction | string | The maintenance margin deduction value when risk limit tier changed |
| nextPageCursor | string | Refer to the cursor request parameter |

---


Request Example

HTTP
 
  
  
  
```http
GET /v5/market/risk-limit?category=inverse&symbol=BTCUSD HTTP/1.1
Host: api-testnet.bybit.com
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "category": "inverse",
        "list": [
            {
                "id": 1,
                "symbol": "BTCUSD",
                "riskLimitValue": "150",
                "maintenanceMargin": "0.5",
                "initialMargin": "1",
                "isLowestRisk": 1,
                "maxLeverage": "100.00",
                "mmDeduction": ""
            },
        ....
        ]
    },
    "retExtinfo
": {},
    "time": 1672054488010
}
```

