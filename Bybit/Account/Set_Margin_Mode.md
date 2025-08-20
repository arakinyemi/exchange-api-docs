# Set Margin Mode
Default is regular margin mode

info

This switch does not work for the inverse trading in UTA1.0, which margin mode is set per symbol. Please use Switch Cross/Isolated Margin

HTTP Request
```http
POST /v5/account/set-margin-mode
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| setMarginMode | true | string | ISOLATED_MARGIN, REGULAR_MARGIN(i.e. Cross margin), PORTFOLIO_MARGIN |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| reasons | array | Object. If requested successfully, it is an empty array |
| > reasonCode | string | Fail reason code |
| > reasonMsg | string | Fail reason msg |

---


Request Example

HTTP
 
  
```http
POST /v5/account/set-margin-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672134396332
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "setMarginMode": "PORTFOLIO_MARGIN"
}
```

Response Example
```json
{
    "retCode": 3400045,
    "retMsg": "Set margin mode failed",
    "result": {
        "reasons": [
            {
                "reasonCode": "3400000",
                "reasonMsg": "Equity needs to be equal to or greater than 1000 USDC"
            }
        ]
    }
}
```

