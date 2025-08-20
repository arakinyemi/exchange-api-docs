# Set Leverage
info


According to the risk limit, leverage affects the maximum position value that can be opened, that is, the greater the leverage, the smaller the maximum position value that can be opened, and vice versa. Learn more


HTTP Request
```http
POST /v5/position/set-leverage
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> UTA2.0, UTA1.0: linear, inverse <br> Classic account: linear, inverse |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| buyLeverage | true | string | [1, max leverage] <br> one-way mode: buyLeverage must be the same as sellLeverage<br> Hedge mode: <br> Classic account & UTA (isolated margin): buyLeverage and sellLeverage can be different; <br> UTA (cross margin): buyLeverage must be the same as sellLeverage |
| sellLeverage | true | string | [1, max leverage] |

---



Response Parameters
None


Request Example

HTTP
 
  
  
```http
POST /v5/position/set-leverage HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672281605082
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "category": "linear",
    "symbol": "BTCUSDT",
    "buyLeverage": "6",
    "sellLeverage": "6"
```

}

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtinfo
": {},
    "time": 1672281607343
}
```


