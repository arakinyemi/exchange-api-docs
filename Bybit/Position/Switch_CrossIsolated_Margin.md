# Switch Cross/Isolated Margin
Select cross margin mode or isolated margin mode per symbol level


HTTP Request
```http
POST /v5/position/switch-isolated
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> UTA2.0: not supported<br> UTA1.0: inverse<br> Classic: linear(USDT Preps), inverse |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| tradeMode | true | integer | 0: cross margin. 1: isolated margin |
| buyLeverage | true | string | The value must be equal to sellLeverage value |
| sellLeverage | true | string | The value must be equal to buyLeverage value |

---



Response Parameters
None


Request Example

HTTP
 
  
  
```http
POST /v5/position/switch-isolated HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN-TYPE: 2
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675248447965
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 121
```

```json
{
    "category": "linear",
    "symbol": "ETHUSDT",
    "tradeMode": 1,
    "buyLeverage": "10",
    "sellLeverage": "10"
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
    "time": 1675248433635
}
```

