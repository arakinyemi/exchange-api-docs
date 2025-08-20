# Set Auto Add Margin
Turn on/off auto-add-margin for isolated margin position


HTTP Request
```http
POST /v5/position/set-auto-add-margin
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> UTA2.0, UTA1.0: linear (USDT Contract, USDC Contract) <br> Classic account: linear (USDT Perps) |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| autoAddMargin | true | integer | Turn on/off. 0: off. 1: on |
| positionIdx | false | integer | Used to identify positions in different position modes. For hedge mode position, this param is required<br> 0: one-way mode <br> 1: hedge-mode Buy side <br> 2: hedge-mode Sell side |

---


Response Parameters
None



Request Example

HTTP
 
  
  
```http
POST /v5/position/set-auto-add-margin HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN-TYPE: 2
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675255134857
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "category": "linear",
    "symbol": "BTCUSDT",
    "autoAddmargin": 1,
    "positionIdx": null
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtinfo
": {},
    "time": 1675255135069
}
```

