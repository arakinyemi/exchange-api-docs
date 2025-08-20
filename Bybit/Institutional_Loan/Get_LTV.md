# Get LTV
Get your loan-to-value (LTV) ratio.


HTTP Request
```http
GET /v5/ins-loan/ltv-convert
```

Request Parameters
None



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| ltvinfo | array | Object |
| > ltv | string | Risk rate <br> ltv is calculated in real time <br> If you have an INS loan, it is highly recommended to query this data every second. Liquidation occurs when it reachs 0.9 (90%) |
| > rst | string | Remaining liquidation time (UTC time in seconds). When it is not triggered, it is displayed as an empty string. |
| > parentUid | string | The designated Risk Unit ID that was used to bind with the INS loan |
| > subAccountUids | array | Bound user ID |
| > unpaidAmount | string | Total debt(USDT) |
| > unpaidinfo |array | Debt details |
| >> token | string | coin |
| >> unpaidQty | string | Unpaid principle |
| >> unpaidInterest | string | Useless field, please ignore this for now |
| > balance | string | Total asset (margin coins converted to USDT). Please read here to understand the calculation |
| > balanceinfo | array | Asset details |
| >> token | string | Margin coin |
| >> price | string | Margin coin price |
| >> qty | string | Margin coin quantity |
| >> convertedAmount | string | Margin conversion amount |

---

Request Example

HTTP
 
  
```http
GET /v5/ins-loan/ltv-convert HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1686638165351
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "ltvinfo
": [
            {
                "ltv": "0.75",
                "rst": "",
                "parentUid": "xxxxx",
                "subAccountUids": [
                    "60568258"
                ],
                "unpaidAmount": "30",
                "unpaidinfo
": [
                    {
                        "token": "USDT",
                        "unpaidQty": "30",
                        "unpaidInterest": "0"
                    }
                ],
                "balance": "40",
                "balanceinfo
": [
                    {
                        "token": "USDT",
                        "price": "1",
                        "qty": "40",
                        "convertedAmount": "40"
                    }
                ]
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1686638166323
}
```

