### POST / Amend multiple orders

Amend incomplete orders in batches. Maximum 20 orders can be amended per request. Request parameters should be passed in the form of an array.

**Rate Limit:** 300 orders per 2 seconds  
**Rate limit rule:** User ID + Instrument ID  
**Permission:** Trade

Rate limit of this endpoint will also be affected by the rules Sub-account rate limit and Fill ratio based sub-account rate limit.

💡 Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Amend order`.

HTTP Request
```
POST /api/v5/trade/amend-batch-orders
```

Request Example

```
POST /api/v5/trade/amend-batch-orders
body
[
    {
        "ordId":"590909308792049444",
        "newSz":"2",
        "instId":"BTC-USDT"
    },
    {
        "ordId":"590909308792049555",
        "newSz":"2",
        "instId":"BTC-USDT"
    }
]
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Instrument ID |
| cxlOnFail | Boolean | No | Whether the order needs to be automatically canceled when the order amendment fails<br>false true, the default is false. |
| ordId | String | Conditional | Order ID<br>Either ordId or clOrdIdis required, if both are passed, ordId will be used. |
| clOrdId | String | Conditional | Client Order ID as assigned by the client |
| reqId | String | No | Client Request ID as assigned by the client for order amendment<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>The response will include the corresponding reqId to help you identify the request if you provide it in the request. |
| newSz | String | Conditional | New quantity after amendment. When amending a partially-filled order, the newSz should include the amount that has been filled. |
| newPx | String | Conditional | New price after amendment. |
| attachAlgoOrds | Array of objects | No | TP/SL information attached when placing order |
| > attachAlgoId | String | Conditional | The order ID of attached TP/SL order. It is required to identity the TP/SL order when amending. It will not be posted to algoId when placing TP/SL order after the general order is filled completely. |
| > attachAlgoClOrdId | String | Conditional | Client-supplied Algo ID when placing order attaching TP/SL<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > newTpTriggerPx | String | Conditional | Take-profit trigger price.<br>Either the take profit trigger price or order price is 0, it means that the take profit is deleted.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newTpOrdPx | String | Conditional | Take-profit order price<br>If the price is -1, take-profit will be executed at the market price.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newSlTriggerPx | String | Conditional | Stop-loss trigger price<br>Either the stop loss trigger price or order price is 0, it means that the stop loss is deleted.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newSlOrdPx | String | Conditional | Stop-loss order price<br>If the price is -1, stop-loss will be executed at the market price.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newTpTriggerPxType | String | Conditional | Take-profit trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>Only applicable to FUTURES/SWAP<br>If you want to add the take-profit, this parameter is required |
| > newSlTriggerPxType | String | Conditional | Stop-loss trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>Only applicable to FUTURES/SWAP<br>If you want to add the stop-loss, this parameter is required |
| > sz | String | Conditional | New size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs |
| > amendPxOnTriggerType | String | No | Whether to enable Cost-price SL. Only applicable to SL order of split TPs.<br>0: disable, the default value<br>1: Enable |

Response Example

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "reqId":"b12344",
            "sCode":"0",
            "sMsg":""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "reqId":"b12344",
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
| > reqId | String | Client Request ID as assigned by the client for order amendment. |
| > sCode | String | The code of the event execution result, 0 means success. |
| > sMsg | String | Rejection message if the request is unsuccessful. |
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. 1597026383085123<br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. 1597026383085123 |

💡 **newSz**  
If the new quantity of the order is less than or equal to the filled quantity when you are amending a partially-filled order, the order status will be changed to filled.
