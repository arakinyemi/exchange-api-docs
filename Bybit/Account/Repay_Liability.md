# Repay Liability
You can manually repay the liabilities of Unified account

Permission: USDC Contracts


HTTP Request
```http
POST /v5/account/quick-repayment
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| coin | false | string | The coin with liability, uppercase only <br> Input the specific coin: repay the liability of this coin in particular <br> No coin specified: repay the liability of all coins |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > coin | string | Coin used for repayment <br> The order of currencies used to repay liability is based on liquidationOrder from this endpoint |
| > repaymentQty | string | Repayment qty |

---

Request Example

HTTP
 
  
```http
POST /v5/account/quick-repayment HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1701848610019
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 22
```

```json
{
    "coin": "USDT"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "SUCCESS",
    "result": {
        "list": [
            {
                "coin": "BTC",
                "repaymentQty": "0.10549670"
            },
            {
                "coin": "ETH",
                "repaymentQty": "2.27768114"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1701848610941
}
```

