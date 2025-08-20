# Set TP/SL Mode 

tip

To some extent, this endpoint is deprecated because now tpsl is based on order level. This API was used for position level change before.

However, you still can use it to set an implicit tpsl mode for a certain symbol because when you don't pass "tpslMode" in the place order or trading stop request, system will get the tpslMode by the default setting.

Set TP/SL mode to Full or Partial

info

For partial TP/SL mode, you can set the TP/SL size smaller than position size.


HTTP Request
```http
POST /v5/position/set-tpsl-mode
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> Unified account: linear, inverse <br> Classic account: linear, inverse. Please note that category is not involved with business logic |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| tpSlMode | true | string | TP/SL mode. Full,Partial |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| tpSlMode | string | Full,Partial |

---


Request Example

HTTP
 
  
  
```http
POST /v5/position/set-tpsl-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672279325035
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "symbol": "XRPUSDT",
    "category": "linear",
    "tpSlMode": "Full"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "tpSlMode": "Full"
    },
    "retExtinfo
": {},
    "time": 1672279322666
}
```


