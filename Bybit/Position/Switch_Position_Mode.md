# Switch Position Mode
It supports to switch the position mode for USDT perpetual and Inverse futures. If you are in one-way Mode, you can only open one position on Buy or Sell side. If you are in hedge mode, you can open both Buy and Sell side positions simultaneously.


tip


Priority for configuration to take effect: symbol > coin > system default

System default: one-way mode

If the request is by coin (settleCoin), then all symbols based on this setteCoin that do not have position and open order will be batch switched, and new listed symbol based on this settleCoin will be the same mode you set.

Example

|                         | System default                                                                                                                                    | coin                   | symbol                    |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|---------------------------|
| Initial setting         | one-way                                                                                                                                           | never configured       | never configured          |
|          Result         |                                                 All USDT perpetual trading pairs are one-way mode                                                 |                        |                           |
| Change 1                | -                                                                                                                                                 | -                      | Set BTCUSDT to hedge-mode |
|          Result         |                                        BTCUSDT becomes hedge-mode, and all other symbols keep one-way mode                                        |                        |                           |
| list new symbol ETHUSDT | ETHUSDT is one-way mode (inherit default rules)                                                                                                   |                        |                           |
| Change 2                | -                                                                                                                                                 | Set USDT to hedge-mode | -                         |
|          Result         | All current trading pairs with no positions or orders are hedge-mode, and no adjustments will be made for trading pairs with positions and orders |                        |                           |
| list new symbol SOLUSDT | SOLUSDT is hedge-mode (Inherit coin rule)                                                                                                         |                        |                           |
| Change 3                | -                                                                                                                                                 | -                      | Set ASXUSDT to one-mode   |
|    Take effect result   |                                            AXSUSDT is one-way mode, other trading pairs have no change                                            |                        |                           |
| list new symbol BITUSDT | BITUSDT is hedge-mode (Inherit coin rule)                                                                                                         |                        |                           |

The position-switch ability for each contract

|  Classic account  |            UTA1.0            |            UTA2.0            |                              |
|:-----------------:|:----------------------------:|:----------------------------:|------------------------------|
| USDT perpetual    | Support one-way & hedge-mode | Support one-way & hedge-mode | Support one-way & hedge-mode |
| USDT futures      | N/A                          | Support one-way only         | Support one-way only         |
| USDC perpetual    | N/A                          | Support one-way only         | Support one-way only         |
| USDC futures      | N/A                          | Support one-way only         | Support one-way only         |
| Inverse perpetual | Support one-way only         | Support one-way only         | Support one-way only         |
| Inverse futures   | Support one-way & hedge-mode | Support one-way & hedge-mode | Support one-way only         |

HHTP Request
```http
POST /v5/position/switch-mode
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> UTA2.0: linear, USDT Contract<br> UTA1.0: linear, USDT Contract; inverse, Inverse Futures<br> Classic: linear, USDT Perp; inverse, Inverse Futures |
| symbol | false | string | Symbol name, like BTCUSDT, uppercase only. Either symbol or coin is required. symbol has a higher priority |
| coin | false | string | Coin, uppercase only |
| mode | true | integer | Position mode. 0: Merged Single. 3: Both Sides |

---



Response Parameters
None


Request Example

HTTP
 
  
  
```http
POST /v5/position/switch-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675249072041
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 87
```

```json
{
    "category":"inverse",
    "symbol":"BTCUSDH23",
    "coin": null,
    "mode": 0
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
    "time": 1675249072814
}
```



