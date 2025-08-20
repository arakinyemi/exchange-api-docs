# Set Limit Price Behaviour
You can configure how the system behaves when your limit order price exceeds the highest bid or lowest ask price.

Spot

Maximum Buy Price: Min[Max(Index, Index × (1 + y%) + 2-Minute Average Premium), Index × (1 + z%)]
Lowest price for Sell: Max[Min(Index, Index × (1 – y%) + 2-Minute Average Premium), Index × (1 – z%)]
Futures

Maximum Buy Price: min( max( index , markprice ( 1 + x% ）), markprice ( 1 + y%) )
Lowest price for Sell: max ( min( index , markprice ( 1 - x% )) , markprice ( 1 - y%) )
Default Setting
Spot: If the order price exceeds the boundary, the system rejects the request.

Futures: If the order price exceeds the boundary, the system will automatically adjust the price to the nearest allowed boundary (i.e., highest bid or lowest ask).


HTTP Request
```http
POST /v5/account/set-limit-px-action
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | linear, inverse, spot |
| modifyEnable | true | boolean | true: allow the syetem to modify the order price <br> false: reject your order request |

---


Response Parameters
None


Request Example

HTTP
 
  
```http
POST /v5/account/set-limit-px-action HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1753255927950
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 52
```

```json
{
    "category": "spot",
    "modifyEnable": true
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {},
    "retExtinfo
": {},
    "time": 1753255927952
}
```

