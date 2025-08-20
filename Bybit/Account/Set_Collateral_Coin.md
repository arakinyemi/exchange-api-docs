# Set Collateral Coin
You can decide whether the assets in the Unified account needs to be collateral coins.


HTTP Request
```http
POST /v5/account/set-collateral-switch
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| coin | true | string | Coin name, uppercase only <br> You can get collateral coin from here <br> USDT, USDC cannot be set |
| collateralSwitch | true | string | ON: switch on collateral, OFF: switch off collateral |

---


Response Parameters
None



Request Example

HTTP
 
  
```http
POST /v5/account/set-collateral-switch HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1690513916181
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 55
```

```json
{
    "coin": "BTC",
    "collateralSwitch": "ON"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "SUCCESS",
    "result": {},
    "retExtinfo
": {},
    "time": 1690515818656
}
```

