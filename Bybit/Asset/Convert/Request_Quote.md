
# Request a Quote

HTTP Request
```http
POST /v5/asset/exchange/quote-apply
```

Request Parameters
| Parameter     | Required | Type   | Comments                                                                                                                                                      |
|---------------|----------|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| accountType   | true     | string | Wallet type                                                                                                                                                   |
| fromCoin      | true     | string | Convert from coin (coin to sell)                                                                                                                              |
| toCoin        | true     | string | Convert to coin (coin to buy)                                                                                                                                 |
| requestCoin   | true     | string | Request coin, same as fromCoin<br>In the future, we may support requestCoin=toCoin                                                                            |
| requestAmount | true     | string | request coin amount (the amount you want to sell)                                                                                                             |
| fromCoinType  | false    | string | crypto                                                                                                                                                        |
| toCoinType    | false    | string | crypto                                                                                                                                                        |
| paramType     | false    | string | opFrom, mainly used for API broker user                                                                                                                       |
| paramValue    | false    | string | Broker ID, mainly used for API broker user                                                                                                                    |
| requestId     | false    | string | Customised request ID<br>a maximum length of 36<br>Generally it is useless, but it is convenient to track the quote request internally if you fill this field |

Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| quoteTxId | string | Quote transaction ID. It is system generated, and it is used to confirm quote and query the result of transaction |
| exchangeRate | string | Exchange rate |
| fromCoin | string | From coin |
| fromCoinType | string | From coin type. crypto |
| toCoin | string | To coin |
| toCoinType | string | To coin type. crypto |
| fromAmount | string | From coin amount (amount to sell) |
| toAmount | string | To coin amount (amount to buy according to exchange rate) |
| expiredTime | string | The expiry time for this quote (15 seconds) |
| requestId | string  | Customised request ID |
| extTaxAndFee	| array	 | Compliance-related field. Currently returns an empty array, which may be used in the future
---

Request Example

HTTP
 
  
```http
POST /v5/asset/exchange/quote-apply HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1720071077014
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
Content-Type: application/json
Content-Length: 172
```

```json
{
    "requestId": "test-00002",
    "fromCoin": "ETH",
    "toCoin": "BTC",
    "accountType": "eb_convert_funding",
    "requestCoin": "ETH",
    "requestAmount": "0.1",
    "paramType": "opFrom",
    "paramValue": "broker-id-001"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "quoteTxId": "10100108106409340067234418688",
        "exchangeRate": "0.053517914861880000",
        "fromCoin": "ETH",
        "fromCoinType": "crypto",
        "toCoin": "BTC",
        "toCoinType": "crypto",
        "fromAmount": "0.1",
        "toAmount": "0.005351791486188000",
        "expiredTime": "1720071092225",
        "requestId": "test-00002",
        "extTaxAndFee":[]
    },
    "retExtinfo
": {},
    "time": 1720071077265
}
```

