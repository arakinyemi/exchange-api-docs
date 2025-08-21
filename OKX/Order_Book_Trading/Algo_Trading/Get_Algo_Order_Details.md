### Get / Algo order details

Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

HTTP Request

`GET /api/v5/trade/order-algo`

Request Example

`GET /api/v5/trade/order-algo?algoId=1753184812254216192`

Request Parameters

| Parameter   | Type   | Required    | Description                             |
|-------------|--------|-------------|-----------------------------------------|
| algoId      | String | Conditional | Algo ID (Either algoId or algoClOrdId is required)|
| algoClOrdId | String | Conditional | Client-supplied Algo ID                 |

Response Example

```
{
    "code": "0",
    "data": [
        {
            "activePx": "",
            "actualPx": "",
            "actualSide": "",
            "actualSz": "0",
            "algoClOrdId": "",
            "algoId": "1753184812254216192",
            "amendPxOnTriggerType": "0",
            "attachAlgoOrds": [],
            "cTime": "1724751378980",
            ...
            "triggerPx": "",
            "triggerPxType": "",
            "triggerTime": "",
            ...
            "tpTriggerPx": "10000",
            "tpTriggerPxType": "last",
            "tpOrdPx": "-1",
            ...
            "state": "live",
            ...
        }
    ],
    "msg": ""
}
```

Response Parameters (selected)

| Parameter        | Type    | Description                                         |
|------------------|---------|-----------------------------------------------------|
| instType         | String  | Instrument type                                    |
| instId           | String  | Instrument ID                                      |
| ccy              | String  | Margin currency (applicable to margin orders)      |
| ordId            | String  | Latest order ID (deprecated soon)                   |
| ordIdList        | Array   | Order ID list (multiple if TP/SL splitting order)  |
| algoId           | String  | Algo ID                                            |
| clOrdId          | String  | Client Order ID                                    |
| sz               | String  | Quantity to buy or sell                            |
| closeFraction    | String  | Fraction of position to close on algo trigger     |
| ordType          | String  | Order type                                        |
| side             | String  | Order side                                        |
| posSide          | String  | Position side                                     |
| tdMode           | String  | Trade mode                                        |
| tgtCcy           | String  | Order quantity unit setting for sz                |
| state            | String  | State (live, pause, effective, canceled, order_failed...) |
| lever            | String  | Leverage (0.01 to 125)                            |
| tpTriggerPx      | String  | Take-profit trigger price                          |
| tpTriggerPxType  | String  | Take-profit trigger price type                     |
| tpOrdPx          | String  | Take-profit order price                            |
| slTriggerPx      | String  | Stop-loss trigger price                            |
| slTriggerPxType  | String  | Stop-loss trigger price type                       |
| slOrdPx          | String  | Stop-loss order price                              |
| triggerPx        | String  | Trigger price                                     |
| triggerPxType    | String  | Trigger price type                                |
| ordPx            | String  | Order price for trigger order                      |
| actualSz         | String  | Actual order quantity                              |
| actualPx         | String  | Actual order price                                 |
| tag              | String  | Order tag                                         |
| actualSide       | String  | Actual trigger side (tp/sl) (only for oco and conditional)|
| triggerTime      | String  | Trigger time (ms)                                 |
| pxVar            | String  | Price ratio (for iceberg/twap orders)             |
| pxSpread         | String  | Price variance (for iceberg/twap orders)          |
| szLimit          | String  | Average amount (for iceberg/twap orders)          |
| pxLimit          | String  | Price limit (for iceberg/twap orders)             |
| timeInterval     | String  | Time interval (for twap orders)                    |
| callbackRatio    | String  | Callback price ratio (for move_order_stop order)  |
| callbackSpread   | String  | Callback price variance (for move_order_stop order)|
| activePx         | String  | Active price (for move_order_stop order)           |
| moveTriggerPx    | String  | Trigger price (for move_order_stop order)          |
| reduceOnly       | String  | Whether order can only reduce position size         |
| quickMgnType     | String  | Quick Margin type                                  |
| last             | String  | Last filled price while placing order               |
| failCode         | String  | Algo order fail reason (empty if not failed)        |
| algoClOrdId      | String  | Client-supplied Algo ID                             |
| amendPxOnTriggerType | String | Enable cost-price SL for split TPs               |
| attachAlgoOrds   | Array   | Attached SL/TP orders info                         |
| cTime            | String  | Creation time (ms)                                 |
| uTime            | String  | Update time (ms)                                   |
| isTradeBorrowMode | String | Whether borrowing currency automatically<br>true<br>false<br>Only applicable to trigger order, trailing order and twap order |
| chaseType | String | Chase type. Only applicable to chase order. |
| chaseVal | String | Chase value. Only applicable to chase order. |
| maxChaseType | String | Maximum chase type. Only applicable to chase order. |
| maxChaseVal | String | Maximum chase value. Only applicable to chase order. |
| tradeQuoteCcy | String | The quote currency used for trading. |
