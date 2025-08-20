# Get Delivery Record
Query delivery records of Invese Futures, USDC Futures, USDT Futures and Options, sorted by deliveryTime in descending order


HTTP Request
```http
GET /v5/asset/delivery-record
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br> UTA2.0: inverse(inverse futures), linear(USDT/USDC futures), option <br> UTA1.0: linear(USDT/USDC futures), option |
| symbol | false | string | Symbol name, like BTCUSDT, uppercase only |
| startTime | false | integer | The start timestamp (ms) <br> startTime and endTime are not passed, return 30 days by default <br> Only startTime is passed, return range between startTime and startTime + 30 days <br> Only endTime is passed, return range between endTime - 30 days and endTime <br> If both are passed, the rule is endTime - startTime <= 30 days |
| endTime | false | integer | The end timestamp (ms) |
| expDate | false | string | Expiry date. 25MAR22. Default: return all |
| limit | false | integer | Limit for data size per page. [1, 50]. Default: 20 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| category | string | Product type |
| list | array | Object |
| > deliveryTime | number | Delivery time (ms) |
| > symbol | string | Symbol name |
| > side | string | Buy,Sell |
| > position | string | Executed size |
| > entryPrice | string | Avg entry price |
| > deliveryPrice | string | Delivery price |
| > strike | string | Exercise price |
| > fee | string | Trading fee |
| > deliveryRpl | string | Realized PnL of the delivery |
| nextPageCursor | string | Refer to the cursor request parameter |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/delivery-record?expDate=29DEC22&category=option HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672362112944
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "132791%3A0%2C132791%3A0",
        "category": "option",
        "list": [
            {
                "symbol": "BTC-29DEC22-16000-P",
                "side": "Buy",
                "deliveryTime": 1672300800860,
                "strike": "16000",
                "fee": "0.00000000",
                "position": "0.01",
                "deliveryPrice": "16541.86369547",
                "deliveryRpl": "3.5"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1672362116184
}
```

