# Get Transaction log (UTA)

Query for transaction logs in your Unified account. It supports up to 2 years worth of data.

Apply to: UTA2.0, UTA1.0(except inverse)

HTTP Request
```http
GET /v5/account/transaction-log
```

Request Parameters
| Parameter   | Required | Type    | Comments                                                                                                                                                                                                                                                                                                                      |
|-------------|----------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| accountType | false    | string  | Account Type. UNIFIED                                                                                                                                                                                                                                                                                                         |
| category    | false    | string  | Product type<br>UTA2.0: spot,linear,option,inverse<br>UTA1.0: spot,linear,option                                                                                                                                                                                                                                              |
| currency    | false    | string  | Currency, uppercase only                                                                                                                                                                                                                                                                                                      |
| baseCoin    | false    | string  | BaseCoin, uppercase only. e.g., BTC of BTCPERP                                                                                                                                                                                                                                                                                |
| type        | false    | string  | Types of transaction logs                                                                                                                                                                                                                                                                                                     |
| startTime   | false    | integer | The start timestamp (ms)<br>startTime and endTime are not passed, return 24 hours by default<br>Only startTime is passed, return range between startTime and startTime+24 hours<br>Only endTime is passed, return range between endTime-24 hours and endTime<br>If both are passed, the rule is endTime - startTime <= 7 days |
| endTime     | false    | integer | The end timestamp (ms)                                                                                                                                                                                                                                                                                                        |
| limit       | false    | integer | Limit for data size per page. [1, 50]. Default: 20                                                                                                                                                                                                                                                                            |
| cursor      | false    | string  | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set                                                                                                                                                                                                                            |


Response Parameters
| Parameter         | Type   |                                                                                                                                                                                        Comments                                                                                                                                                                                        |
|-------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| list              | array  | Object                                                                                                                                                                                                                                                                                                                                                                                 |
| > id              | string | Unique id                                                                                                                                                                                                                                                                                                                                                                              |
| > symbol          | string | Symbol name                                                                                                                                                                                                                                                                                                                                                                            |
| > category        | string | Product type                                                                                                                                                                                                                                                                                                                                                                           |
| > side            | string | Side. Buy,Sell,None                                                                                                                                                                                                                                                                                                                                                                    |
| > transactionTime | string | Transaction timestamp (ms)                                                                                                                                                                                                                                                                                                                                                             |
| > type            | string | Type                                                                                                                                                                                                                                                                                                                                                                                   |
| > transSubType    | string | Transaction sub type, movePosition, used for the logs generated by move position. "" by default                                                                                                                                                                                                                                                                                        |
| > qty             | string | Quantity<br>Spot: the negative means the qty of this currency is decreased, the positive means the qty of this currency is increased<br>Perps & Futures: it is the quantity for each trade entry and it does not have direction                                                                                                                                                        |
| > size            | string | Size. The rest position size after the trade is executed, and it has direction, i.e., short with "-"                                                                                                                                                                                                                                                                                   |
| > currency        | string | e.g., USDC, USDT, BTC, ETH                                                                                                                                                                                                                                                                                                                                                             |
| > tradePrice      | string | Trade price                                                                                                                                                                                                                                                                                                                                                                            |
| > funding         | string | Funding fee<br>Positive fee value means an expense; negative fee value means a rebate. This is opposite to the execFee from Get Trade History.<br>For USDC Perp, as funding settlement and session settlement occur at the same time, they are represented in a single record at settlement. Please refer to funding to understand funding fee, and cashFlow to understand 8-hour P&L. |
| > fee             | string | Trading fee<br>Positive fee value means expense<br>Negative fee value means rebates                                                                                                                                                                                                                                                                                                    |
| > cashFlow        | string | Cash flow, e.g., (1) close the position, and unRPL converts to RPL, (2) 8-hour session settlement for USDC Perp and Futures, (3) transfer in or transfer out. This does not include trading fee, funding fee                                                                                                                                                                           |
| > change          | string | Change = cashFlow + funding - fee                                                                                                                                                                                                                                                                                                                                                      |
| > cashBalance     | string | Cash balance. This is the wallet balance after a cash change                                                                                                                                                                                                                                                                                                                           |
| > feeRate         | string | When type=TRADE, then it is trading fee rate<br>When type=SETTLEMENT, it means funding fee rate. For side=Buy, feeRate=market fee rate; For side=Sell, feeRate= - market fee rate                                                                                                                                                                                                      |
| > bonusChange     | string | The change of bonus                                                                                                                                                                                                                                                                                                                                                                    |
| > tradeId         | string | Trade ID                                                                                                                                                                                                                                                                                                                                                                               |
| > orderId         | string | Order ID                                                                                                                                                                                                                                                                                                                                                                               |
| > orderLinkId     | string | User customised order ID                                                                                                                                                                                                                                                                                                                                                               |
| > extraFees       | string | Trading fee rate information. Currently, this data is returned only for spot orders placed on the Indonesian site or spot fiat currency orders placed on the EU site. In other cases, an empty string is returned. Enum: feeType, subFeeType                                                                                                                                           |
| nextPageCursor    | string | Refer to the cursor request parameter                                                                                                                                                                                                                                                                                                                                                  |


Request Example
HTTP
```http
GET /v5/account/transaction-log?accountType=UNIFIED&category=linear&currency=USDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672132480085
X-BAPI-RECV-WINDOW: 5000
```
Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "21963%3A1%2C14954%3A1",
        "list": [
            {
                "transSubType": "",
                "id": "592324_XRPUSDT_161440249321",
                "symbol": "XRPUSDT",
                "side": "Buy",
                "funding": "-0.003676",
                "orderLinkId": "",
                "orderId": "1672128000-8-592324-1-2",
                "fee": "0.00000000",
                "change": "-0.003676",
                "cashFlow": "0",
                "transactionTime": "1672128000000",
                "type": "SETTLEMENT",
                "feeRate": "0.0001",
                "bonusChange": "",
                "size": "100",
                "qty": "100",
                "cashBalance": "5086.55825002",
                "currency": "USDT",
                "category": "linear",
                "tradePrice": "0.3676",
                "tradeId": "534c0003-4bf7-486f-aa02-78cee36825e4",
                "extraFees": ""
            },
            {
                "transSubType": "",
                "id": "592324_XRPUSDT_161440249321",
                "symbol": "XRPUSDT",
                "side": "Buy",
                "funding": "",
                "orderLinkId": "linear-order",
                "orderId": "592b7e41-78fd-42e2-9aa3-91e1835ef3e1",
                "fee": "0.01908720",
                "change": "-0.0190872",
                "cashFlow": "0",
                "transactionTime": "1672121182224",
                "type": "TRADE",
                "feeRate": "0.0006",
                "bonusChange": "-0.1430544",
                "size": "100",
                "qty": "88",
                "cashBalance": "5086.56192602",
                "currency": "USDT",
                "category": "linear",
                "tradePrice": "0.3615",
                "tradeId": "5184f079-88ec-54c7-8774-5173cafd2b4e",
                "extraFees": ""
            },
            {
                "transSubType": "",
                "id": "592324_XRPUSDT_161407743011",
                "symbol": "XRPUSDT",
                "side": "Buy",
                "funding": "",
                "orderLinkId": "linear-order",
                "orderId": "592b7e41-78fd-42e2-9aa3-91e1835ef3e1",
                "fee": "0.00260280",
                "change": "-0.0026028",
                "cashFlow": "0",
                "transactionTime": "1672121182224",
                "type": "TRADE",
                "feeRate": "0.0006",
                "bonusChange": "",
                "size": "12",
                "qty": "12",
                "cashBalance": "5086.58101322",
                "currency": "USDT",
                "category": "linear",
                "tradePrice": "0.3615",
                "tradeId": "8569c10f-5061-5891-81c4-a54929847eb3",
                "extraFees": ""
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672132481405
}
```