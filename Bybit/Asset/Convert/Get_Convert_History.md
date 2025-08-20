# Get Convert History
Returns all confirmed quotes.

info

Only displays the conversion history of conversions created through the API.


HTTP Request
```http
GET /v5/asset/exchange/query-convert-history
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| accountType | false | string | Wallet type <br> Supports passing multiple types, separated by comma e.g., eb_convert_funding,eb_convert_uta <br> Return all wallet types data if not passed |
| index | false | integer | Page number <br> started from 1 <br> 1st page by default |
| limit | false | integer | Page size <br> 20 records by default <br> up to 100 records, return 100 when exceeds 100 |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array`<object>` | Array of quotes |
| > accountType | string | Wallet type |
| > exchangeTxId | string | Exchange tx ID, same as quote tx ID |
| > userId | string | User ID |
| > fromCoin | string | From coin |
| > fromCoinType | string | From coin type. crypto |
| > toCoin | string | To coin |
| > toCoinType | string | To coin type. crypto |
| > fromAmount | string | From coin amount (amount to sell) |
| > toAmount | string | To coin amount (amount to buy according to exchange rate) |
| > exchangeStatus | string | Exchange status <br> init <br> processing <br> success <br> failure |
| > extinfo | object |
| >> paramType | string | This field is published when you send it in the Request a Quote |
| >> paramValue | string | This field is published when you send it in the Request a Quote |
| > convertRate | string | Exchange rate |
| > createdAt | string | Quote created time |

---

Request Example

HTTP
 
  
```http
GET /v5/asset/exchange/query-convert-history?accountType=eb_convert_uta,eb_convert_funding HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1720074159814
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
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
": {
                    "paramType": "opFrom",
                    "paramValue": "broker-id-001"
                },
                "convertRate": "0.0534882723991",
                "createdAt": "1720071899995"
            },
            {
                "accountType": "eb_convert_uta",
                "exchangeTxId": "23070eb_convert_uta408933875189391360",
                "userId": "XXXXX",
                "fromCoin": "BTC",
                "fromCoinType": "crypto",
                "fromAmount": "0.1",
                "toCoin": "ETH",
                "toCoinType": "crypto",
                "toAmount": "1.773938248611074",
                "exchangeStatus": "success",
                "extinfo
": {},
                "convertRate": "17.73938248611074",
                "createdAt": "1719974243256"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1720074457715
}
```

