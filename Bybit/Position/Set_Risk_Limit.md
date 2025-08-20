# Set Risk Limit
Since bybit has launched auto risk limit on 12 March 2024, please click here to learn more, so it will not take effect even you set it successfully.


HTTP Request
```http
POST /v5/position/set-risk-limit
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> Unified account: linear, inverse <br> Classic account: linear, inverse. Please note that category is not involved with business logic |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| riskId | true | integer | Risk limit ID |
| positionIdx | false | integer | Used to identify positions in different position modes. For hedge mode, it is required <br> 0: one-way mode <br> 1: hedge-mode Buy side<br> 2: hedge-mode Sell side |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type |
| riskId | integer | Risk limit ID |
| riskLimitValue | string | The position limit value corresponding to this risk ID |

---


Request Example

HTTP
 
  
  
```http
POST /v5/position/set-risk-limit HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672282269774
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "category": "linear",
    "symbol": "BTCUSDT",
    "riskId": 4,
    "positionIdx": null
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "riskId": 4,
        "riskLimitValue": "8000000",
        "category": "linear"
    },
    "retExtinfo
": {},
    "time": 1672282270571
}
```
