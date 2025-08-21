### POST / Place multiple orders

Place orders in batches. Maximum 20 orders can be placed per request.  
Request parameters should be passed in the form of an array. Orders will be placed in turn

**Rate Limit:** 300 orders per 2 seconds  
**Rate limit rule:** User ID + Instrument ID  
**Permission:** Trade

💡 Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Place order`.

HTTP Request
```
POST /api/v5/trade/batch-orders
```

Request Example

```
# batch place order for SPOT
POST /api/v5/trade/batch-orders
body
[
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b15",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    },
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b16",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    }
]
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Instrument ID, e.g. BTC-USDT |
| tdMode | String | Yes | Trade mode<br>cash |
| clOrdId | String | No | Client Order ID as assigned by the client<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag | String | No | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters. |
| side | String | Yes | Order side<br>buy<br>sell |
| ordType | String | Yes | Order type<br>market: Market order<br>limit: Limit order<br>post_only: Post-only order<br>fok: Fill-or-kill order<br>ioc: Immediate-or-cancel order |
| sz | String | Yes | Quantity to buy or sell |
| px | String | Conditional | Order price. Only applicable to limit,post_only,fok,iocorder. |
| tgtCcy | String | No | Whether the target currency uses the quote or base currency.<br>base_ccy: Base currency ,quote_ccy: Quote currency<br>Only applicable to SPOT Market Orders<br>Default is quote_ccy for buy, base_ccy for sell |
| banAmend | Boolean | No | Whether to disallow the system from amending the size of the SPOT Market Order.<br>Valid options: true or false. The default value is false.<br>If true, system will not amend and reject the market order if user does not have sufficient funds.<br>Only applicable to SPOT Market Orders |
| stpId | String | No | Self trade prevention ID. Orders from the same master account with the same ID will be prevented from self trade.<br>Numerical integers defined by user in the range of 1<= x<= 999999999 (deprecated) |
| stpMode | String | No | Self trade prevention mode.<br>Default to cancel maker<br>cancel_maker,cancel_taker, cancel_both<br>Cancel both does not support FOK. |
| tradeQuoteCcy | String | No | The quote currency used for trading. Only applicable to SPOT.<br>The default value is the quote currency of the instId, for example: for BTC-USD, the default is USD. |
| attachAlgoOrds | Array of objects | No | TP/SL information attached when placing order |
| > attachAlgoClOrdId | String | No | Client-supplied Algo ID when placing order attaching TP/SL<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > tpTriggerPx | String | Conditional | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well. |
| > tpOrdPx | String | Conditional | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well.<br>If the price is -1, take-profit will be executed at the market price. |
| > slTriggerPx | String | Conditional | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price. |
| > slOrdPx | String | Conditional | Stop-loss order price<br>If you fill in this parameter, you should fill in the stop-loss trigger price.<br>If the price is -1, stop-loss will be executed at the market price. |
| > tpTriggerPxType | String | No | Take-profit trigger price type<br>last: last price<br>The default is last |
| > slTriggerPxType | String | No | Stop-loss trigger price type<br>last: last price<br>The default is last |
| > sz | String | Conditional | Size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs |
| > amendPxOnTriggerType | String | No | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. Whether slTriggerPx will move to avgPx when the first TP order is triggered<br>0: disable, the default value<br>1: Enable |

Response Example

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "tag":"",
            "sCode":"0",
            "sMsg":""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "tag":"",
            "sCode":"0",
            "sMsg":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| code | String | The result code, 0 means success |
| msg | String | The error message, empty if the code is 0 |
| data | Array of objects | Array of objects contains the response results |
| > ordId | String | Order ID |
| > clOrdId | String | Client Order ID as assigned by the client |
| > tag | String | Order tag |
| > sCode | String | The code of the event execution result, 0 means success. |
| > sMsg | String | Rejection or success message of event execution. |
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. 1597026383085123<br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. 1597026383085123 |

💡 **clOrdId**  
clOrdId is a user-defined unique ID used to identify the order. It will be included in the response parameters if you have specified during order submission, and can be used as a request parameter to the endpoints to query, cancel and amend orders.  
clOrdId must be unique among all pending orders and the current request.
