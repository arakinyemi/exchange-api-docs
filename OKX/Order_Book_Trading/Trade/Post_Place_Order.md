### POST / Place order

You can place an order only if you have sufficient funds.

**Rate Limit:** 60 requests per 2 seconds  
**Rate limit rule:** User ID + Instrument ID  
**Permission:** Trade

HTTP Request
```
POST /api/v5/trade/order
```

Request Example

```
# place order for SPOT
POST /api/v5/trade/order
body
{
    "instId":"BTC-USDT",
    "tdMode":"cash",
    "clOrdId":"b15",
    "side":"buy",
    "ordType":"limit",
    "px":"2.15",
    "sz":"2"
}
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
| tgtCcy | String | No | Whether the target currency uses the quote or base currency.<br>base_ccy: Base currency<br>quote_ccy: Quote currency<br>Only applicable to SPOT Market Orders<br>Default is quote_ccy for buy, base_ccy for sell |
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
  "code": "0",
  "msg": "",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "312269865356374016",
      "tag": "",
      "sCode": "0",
      "sMsg": ""
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
clOrdId must be unique among the clOrdIds of all pending orders.

💡 **ordType**  
Order type. When creating a new order, you must specify the order type. The order type you specify will affect: 1) what order parameters are required, and 2) how the matching system executes your order. The following are valid order types:
- **limit**: Limit order, which requires specified sz and px.
- **market**: Market order. For SPOT and MARGIN, market order will be filled with market price (by swiping opposite order book). For Expiry Futures and Perpetual Futures, market order will be placed to order book with most aggressive price allowed by Price Limit Mechanism. For OPTION, market order is not supported yet. As the filled price for market orders cannot be determined in advance, OKX reserves/freezes your quote currency by an additional 5% for risk check.
- **post_only**: Post-only order, which the order can only provide liquidity to the market and be a maker. If the order would have executed on placement, it will be canceled instead.
- **fok**: Fill or kill order. If the order cannot be fully filled, the order will be canceled. The order would not be partially filled.
- **ioc**: Immediate or cancel order. Immediately execute the transaction at the order price, cancel the remaining unfilled quantity of the order, and the order quantity will not be displayed in the order book.
- **optimal_limit_ioc**: Market order with ioc (immediate or cancel). Immediately execute the transaction of this market order, cancel the remaining unfilled quantity of the order, and the order quantity will not be displayed in the order book. Only applicable to Expiry Futures and Perpetual Futures.

💡 **sz**  
Quantity to buy or sell.
- For SPOT Buy and Sell Limit Orders, it refers to the quantity in base currency.
- For SPOT Buy Market Orders, it refers to the quantity in quote currency.
- For SPOT Sell Market Orders, it refers to the quantity in base currency.
- For SPOT Market Orders, it is set by tgtCcy.

💡 **tgtCcy**  
This parameter is used to specify the order quantity in the order request is denominated in the quantity of base or quote currency. This is applicable to SPOT Market Orders only.
- Base currency: base_ccy
- Quote currency: quote_ccy

If you use the Base Currency quantity for buy market orders or the Quote Currency for sell market orders, please note:
1. If the quantity you enter is greater than what you can buy or sell, the system will execute the order according to your maximum buyable or sellable quantity. If you want to trade according to the specified quantity, you should use Limit orders.
2. When the market price is too volatile, the locked balance may not be sufficient to buy the Base Currency quantity or sell to receive the Quote Currency that you specified. We will change the quantity of the order to execute the order based on best effort principle based on your account balance. In addition, we will try to over lock a fraction of your balance to avoid changing the order quantity.

2.1 Example of base currency buy market order:
Taking the market order to buy 10 LTCs as an example, and the user can buy 11 LTC. At this time, if 10 < 11, the order is accepted. When the LTC-USDT market price is 200, and the locked balance of the user is 3,000 USDT, as 200*10 < 3,000, the market order of 10 LTC is fully executed; If the market is too volatile and the LTC-USDT market price becomes 400, 400*10 > 3,000, the user's locked balance is not sufficient to buy using the specified amount of base currency, the user's maximum locked balance of 3,000 USDT will be used to settle the trade. Final transaction quantity becomes 3,000/400 = 7.5 LTC.

2.2 Example of quote currency sell market order:
Taking the market order to sell 1,000 USDT as an example, and the user can sell 1,200 USDT, 1,000 < 1,200, the order is accepted. When the LTC-USDT market price is 200, and the locked balance of the user is 6 LTC, as 1,000/200 < 6, the market order of 1,000 USDT is fully executed; If the market is too volatile and the LTC-USDT market price becomes 100, 100*6 < 1,000, the user's locked balance is not sufficient to sell using the specified amount of quote currency, the user's maximum locked balance of 6 LTC will be used to settle the trade. Final transaction quantity becomes 6 * 100 = 600 USDT.

💡 For placing order with TP/Sl, TP/SL algo order will be generated only when this order is filled fully, or there is no TP/SL algo order generated.

💡 **Mandatory self trade prevention (STP)**  
The trading platform imposes mandatory self trade prevention at master account level, which means the accounts under the same master account, including master account itself and all its affiliated sub-accounts, will be prevented from self trade. The default STP mode is `Cancel Maker`. Users can also utilize the stpMode request parameter of the placing order endpoint to determine the stpMode of a certain order.  
Mandatory self trade prevention will not lead to latency.

There are three STP modes. The STP mode is always taken based on the configuration in the taker order.
1. **Cancel Maker**: This is the default STP mode, which cancels the maker order to prevent self-trading. Then, the taker order continues to match with the next order based on the order book priority.
2. **Cancel Taker**: The taker order is canceled to prevent self-trading. If the user's own maker order is lower in the order book priority, the taker order is partially filled and then canceled. FOK orders are always honored and canceled if they would result in self-trading.
3. **Cancel Both**: Both taker and maker orders are canceled to prevent self-trading. If the user's own maker order is lower in the order book priority, the taker order is partially filled. Then, the remaining quantity of the taker order and the first maker order are canceled. FOK orders are not supported in this mode.
