# Get Convert Status
You can query the exchange result by sending quoteTxId.


HTTP Request
```http
GET /v5/asset/exchange/convert-result-query
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| quoteTxId | true | string | Quote tx ID |
| accountType | true | string | Wallet type |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| result | object |
| > accountType | string | Wallet type |
| > exchangeTxId | string | Exchange tx ID, same as quote tx ID |
| > userId | string | User ID |
| > fromCoin | string | From coin |
| > fromCoinType | string | From coin type. crypto |
| > toCoin | string | To coin |
| > toCoinType | string | To coin type. crypto |
| > fromAmount | string | From coin amount (amount to sell) |
| > toAmount | string | To coin amount (amount to buy according to exchange rate) |
| > exchangeStatus | string | Exchange status <br>init <br> processing <br> success <br> failure |
| > extinfo | object | Reserved field, ignored for now |
| > convertRate | string | Exchange rate |
| > createdAt | string | Quote created time |

---

Request Example

HTTP
 
  
```http
GET /v5/asset/exchange/convert-result-query?quoteTxId=10100108106409343501030232064&accountType=eb_convert_funding HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1720073659847
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "result": {
            "accountType": "eb_convert_funding",
            "exchangeTxId": "10100108106409343501030232064",
            "userId": "XXXXX",
            "fromCoin": "ETH",
            "fromCoinType": "crypto",
            "fromAmount": "0.1",
            "toCoin": "BTC",
            "toCoinType": "crypto",
            "toAmount": "0.00534882723991",
            "exchangeStatus": "success",
            "extinfo
": {},
            "convertRate": "0.0534882723991",
            "createdAt": "1720071899995"
        }
    },
    "retExtinfo
": {},
    "time": 1720073660696
}
```

