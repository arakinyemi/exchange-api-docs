# Get Trade History
Query users' execution records, sorted by execTime in descending order. However, for Classic spot, they are sorted by execId in descending order.


tip


Response items will have sorting issues When 'execTime' is the same, it is recommended to sort according to execId+OrderId+leavesQty. This issue is currently being optimized and will be released. If you want to receive real-time execution information, Use the websocket stream (recommended).

You may have mul
tip
le executions in a single order.

You can query by symbol, baseCoin, orderId and orderLinkId, and if you pass mul
tip
le params, the system will process them according to this priority: orderId > orderLinkId > symbol > baseCoin. orderId and orderLinkId have a higher priority and as long as these two parameters are in the input parameters, other input parameters will be ignored.

info

Unified account supports getting the past 730 days historical trades data


HTTP Request
```http
GET /v5/execution/list
```

Request Parameters
| Parameter               | Required | Type                                                                                                                                                                                                                                         |                                                                                                                                                         Comments                                                                                                                                                         |
|-------------------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| category                | true     | string                                                                                                                                                                                                                                       | Product type<br>UTA2.0, UTA1.0: linear, inverse, spot, option<br>classic account: linear, inverse, spot                                                                                                                                                                                                                  |
| symbol                  | false    | string                                                                                                                                                                                                                                       | Symbol name, like BTCUSDT, uppercase only                                                                                                                                                                                                                                                                                |
| orderId                 | false    | string                                                                                                                                                                                                                                       | Order ID                                                                                                                                                                                                                                                                                                                 |
| orderLinkId             | false    | string                                                                                                                                                                                                                                       | User customised order ID. Classic account does not support this param                                                                                                                                                                                                                                                    |
| baseCoin                | false    | string                                                                                                                                                                                                                                       | Base coin, uppercase only<br>UTA1.0(category=inverse) and classic account are not supported                                                                                                                                                                                                                              |
| startTime               | false    | integer                                                                                                                                                                                                                                      | The start timestamp (ms)<br>startTime and endTime are not passed, return 7 days by default;<br><br>Only startTime is passed, return range between startTime and startTime+7 days<br>Only endTime is passed, return range between endTime-7 days and endTimeIf both are passed, the rule is endTime - startTime <= 7 days |
| endTime                 | false    | integer                                                                                                                                                                                                                                      | The end timestamp (ms)                                                                                                                                                                                                                                                                                                   |
| execType                | false    | string                                                                                                                                                                                                                                       | Execution type. Classic spot is not supported                                                                                                                                                                                                                                                                            |
| limit                   | false    | integer                                                                                                                                                                                                                                      | Limit for data size per page. [1, 100]. Default: 50                                                                                                                                                                                                                                                                      |
|cursor | false | string                                                                                                                                                                                                                                       | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set                                                                                                                                                                                                                       |

---


Response Parameters
| Parameter         | Type    | Comments                                                                                                                                                                                                                                                                                         |
|-------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| category          | string  | Product type                                                                                                                                                                                                                                                                                     |
| list              | array   | Object                                                                                                                                                                                                                                                                                           |
| > symbol          | string  | Symbol name                                                                                                                                                                                                                                                                                      |
| > orderId         | string  | Order ID                                                                                                                                                                                                                                                                                         |
| > orderLinkId     | string  | User customized order ID. Classic spot is not supported                                                                                                                                                                                                                                          |
| > side            | string  | Side. Buy,Sell                                                                                                                                                                                                                                                                                   |
| > orderPrice      | string  | Order price                                                                                                                                                                                                                                                                                      |
| > orderQty        | string  | Order qty                                                                                                                                                                                                                                                                                        |
| > leavesQty       | string  | The remaining qty not executed. Classic spot is not supported                                                                                                                                                                                                                                    |
| > createType      | string  | Order create type<br>classic account & UTA1.0(category=inverse): always ""<br>Spot, Option do not have this key                                                                                                                                                                                  |
| > orderType       | string  | Order type. Market,Limit                                                                                                                                                                                                                                                                         |
| > stopOrderType   | string  | Stop order type. If the order is not stop order, it either returns UNKNOWN or "". Classic spot is not supported                                                                                                                                                                                  |
| > execFee         | string  | Executed trading fee. You can get spot fee currency instruction here                                                                                                                                                                                                                             |
| > execFeeV2       | string  | Spot leg transaction fee, only works for execType=FutureSpread                                                                                                                                                                                                                                   |
| > execId          | string  | Execution ID                                                                                                                                                                                                                                                                                     |
| > execPrice       | string  | Execution price                                                                                                                                                                                                                                                                                  |
| > execQty         | string  | Execution qty                                                                                                                                                                                                                                                                                    |
| > execType        | string  | Executed type. Classic spot is not supported                                                                                                                                                                                                                                                     |
| > execValue       | string  | Executed order value. Classic spot is not supported                                                                                                                                                                                                                                              |
| > execTime        | string  | Executed timestamp (ms)                                                                                                                                                                                                                                                                          |
| > feeCurrency     | string  | Spot trading fee currency Classic spot is not supported                                                                                                                                                                                                                                          |
| > isMaker         | boolean | Is maker order. true: maker, false: taker                                                                                                                                                                                                                                                        |
| > feeRate         | string  | Trading fee rate. Classic spot is not supported                                                                                                                                                                                                                                                  |
| > tradeIv         | string  | Implied volatility. Valid for option                                                                                                                                                                                                                                                             |
| > markIv          | string  | Implied volatility of mark price. Valid for option                                                                                                                                                                                                                                               |
| > markPrice       | string  | The mark price of the symbol when executing. Classic spot is not supported                                                                                                                                                                                                                       |
| > indexPrice      | string  | The index price of the symbol when executing. Valid for option only                                                                                                                                                                                                                              |
| > underlyingPrice | string  | The underlying price of the symbol when executing. Valid for option                                                                                                                                                                                                                              |
| > blockTradeId    | string  | Paradigm block trade ID                                                                                                                                                                                                                                                                          |
| > closedSize      | string  | Closed position size                                                                                                                                                                                                                                                                             |
| > seq             | long    | Cross sequence, used to associate each fill and each position update<br>The seq will be the same when conclude mul
tip
le transactions at the same time<br>Different symbols may have the same seq, please use seq + symbol to check unique<br>classic account Spot trade does not have this field |
| > extraFees       | string  | Trading fee rate information. Currently, this data is returned only for kyc=Indian user or spot orders placed on the Indonesian site or spot fiat currency orders placed on the EU site. In other cases, an empty string is returned. Enum: feeType, subFeeType                                  |
| nextPageCursor    | string  | Refer to the cursor request parameter                                                                                                                                                                                                                                                            |

---

Request Example
```http
GET /v5/execution/list?category=linear&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672283754132
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "132766%3A2%2C132766%3A2",
        "category": "linear",
        "list": [
            {
                "symbol": "ETHPERP",
                "orderType": "Market",
                "underlyingPrice": "",
                "orderLinkId": "",
                "side": "Buy",
                "indexPrice": "",
                "orderId": "8c065341-7b52-4ca9-ac2c-37e31ac55c94",
                "stopOrderType": "UNKNOWN",
                "leavesQty": "0",
                "execTime": "1672282722429",
                "feeCurrency": "",
                "isMaker": false,
                "execFee": "0.071409",
                "feeRate": "0.0006",
                "execId": "e0cbe81d-0f18-5866-9415-cf319b5dab3b",
                "tradeIv": "",
                "blockTradeId": "",
                "markPrice": "1183.54",
                "execPrice": "1190.15",
                "markIv": "",
                "orderQty": "0.1",
                "orderPrice": "1236.9",
                "execValue": "119.015",
                "execType": "Trade",
                "execQty": "0.1",
                "closedSize": "",
                "extraFees": "",
                "seq": 4688002127
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1672283754510
}
```

