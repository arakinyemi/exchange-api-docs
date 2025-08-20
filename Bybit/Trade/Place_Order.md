# Place Order
This endpoint supports to create the order for Spot, Margin trading, USDT perpetual, USDT futures, USDC perpetual, USDC futures, Inverse Futures and Options.

info

Supported order type (orderType):
Limit order: orderType=Limit, it is necessary to specify order qty and price.

Market order: orderType=Market, execute at the best price in the Bybit market until the transaction is completed. When selecting a market order, the "price" can be empty. In the futures trading system, in order to protect traders against the serious slippage of the Market order, Bybit trading engine will convert the market order into an IOC limit order for matching. If there are no orderbook entries within price slippage limit, the order will not be executed. If there is insufficient liquidity, the order will be cancelled. The slippage threshold refers to the percentage that the order price deviates from the mark price. You can learn more here: Adjustments to Bybit's Derivative Trading Price Limit Mechanism

Supported timeinfo
rce strategy:
GTC
IOC
FOK

PostOnly: If the order would be filled immediately when submitted, it will be cancelled. The purpose of this is to protect your order during the submission process. If the matching system cannot entrust the order to the order book due to price changes on the market, it will be cancelled.
RPI: Retail Price Improvement order. Assigned market maker can place this kind of order, and it is a post only order, only match with the order from Web or APP.

How to create a conditional order:
When submitting an order, if triggerPrice is set, the order will be automatically converted into a conditional order. In addition, the conditional order does not occupy the margin. If the margin is insufficient after the conditional order is triggered, the order will be cancelled.

Take profit / Stop loss: You can set TP/SL while placing orders. Besides, you could modify the position's TP/SL.

Order quantity: The quantity of perpetual contracts you are   ing to buy/sell. For the order quantity, Bybit only supports positive number at present.

Order price: Place a limit order, this parameter is required. If you have position, the price should be higher than the liquidation price. For the minimum unit of the price change, please refer to the priceFilter > tickSize field in the instruments-info
 endpoint.

orderLinkId: You can customize the active order ID. We can link this ID to the order ID in the system. Once the active order is successfully created, we will send the unique order ID in the system to you. Then, you can use this order ID to cancel active orders, and if both orderId and orderLinkId are entered in the parameter input, Bybit will prioritize the orderId to process the corresponding order. Meanwhile, your customized order ID should be no longer than 36 characters and should be unique.

Open orders up limit:
Perps & Futures:
a) Each account can hold a maximum of 500 active orders simultaneously per symbol.
b) conditional orders: each account can hold a maximum of 10 active orders simultaneously per symbol.
Spot: 500 orders in total, including a maximum of 30 open TP/SL orders, a maximum of 30 open conditional orders for each symbol per account
Option: a maximum of 50 open orders per account

Rate limit:
Please refer to rate limit table. If you need to raise the rate limit, please contact your client manager or submit an application via here

Risk control limit notice:
Bybit will monitor on your API requests. When the total number of orders of a single user (aggregated the number of orders across main account and subaccounts) within a day (UTC 0 - UTC 24) exceeds a certain upper limit, the platform will reserve the right to remind, warn, and impose necessary restrictions. Customers who use API default to acceptance of these terms and have the obligation to cooperate with adjustments.

Spot Stop Order
Spot supports TP/SL order, Conditional order, however, the system logic is different between classic account and Unified account
classic account: When the stop order is created, you will get an order ID. After it is triggered, you will get a new order ID
Unified account: When the stop order is created, you will get an order ID. After it is triggered, the order ID will not be changed


HTTP Request
```http
POST /v5/order/create
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| category | true | string | Product type <br>UTA2.0, UTA1.0: linear, inverse, spot, option <br> classic account: linear, inverse, spot |
| symbol | true | string | Symbol name, like BTCUSDT, uppercase only |
| isLeverage | false | integer | Whether to borrow. Unified account Spot trading only. <br> 0(default): false, spot trading <br> 1: true, margin trading, make sure you turn on margin trading, and set the relevant currency as collateral |
| side | true | string | Buy, Sell |
| orderType | true | string | Market, Limit |
| qty | true | string | Order quantity <br> > UTA account <br> >> Spot: Market Buy order by value by default, you can set marketUnit field to choose order by value or qty for market orders <br>>> Perps, Futures & Option: always order by qty <br> > classic account <br> >>Spot: Market Buy order by value by default <br> >> Perps, Futures: always order by qty <br> > Perps & Futures: if you pass qty="0" and specify reduceOnly=true&closeOnTrigger=true, you can close the position up to maxMktOrderQty or maxOrderQty shown on Get Instruments info
 of current symbol |
| marketUnit | false | string | Select the unit for qty when create Spot market orders for UTA account <br> baseCoin: for example, buy BTCUSDT, then "qty" unit is BTC <br> quoteCoin: for example, sell BTCUSDT, then "qty" unit is USDT |
| slippageToleranceType | false | string | Slippage tolerance Type for market order, TickSize, Percent <br> Support linear, inverse, spot trading, but take profit, stoploss, conditional orders are not supported <br> TickSize:<br> the highest price of Buy order = ask1 + slippageTolerance x tickSize; <br> the lowest price of Sell order = bid1 - slippageTolerance x tickSize <br> Percent: <br> the highest price of Buy order = ask1 x (1 + slippageTolerance x 0.01); <br> the lowest price of Sell order = bid1 x (1 - slippageTolerance x 0.01) |
| slippageTolerance | false | string | Slippage tolerance value <br> TickSize: range is [5, 2000], integer only <br> Percent: range is [0.05, 1], up to 2 decimals |
| price | false | string | Order price <br> Market order will ignore this field <br> Please check the min price and price precision from instrument info
 endpoint <br> If you have position, price needs to be better than liquidation price |
| triggerDirection | false | integer | Conditional order param. Used to identify the expected direction of the conditional order.<br> 1: triggered when market price rises to triggerPrice <br> 2: triggered when market price falls to triggerPrice <br> Valid for linear & inverse |
| orderFilter | false | string | If it is not passed, Order by default. <br> Order <br> tpslOrder: Spot TP/SL order, the assets are occupied even before the order is triggered <br> StopOrder: Spot conditional order, the assets will not be occupied until the price of the underlying asset reaches the trigger price, and the required assets will be occupied after the Conditional order is triggered <br> Valid for spot only |
| triggerPrice | false | string |For Perps & Futures, it is the conditional order trigger price. If you expect the price to rise to trigger your conditional order, make sure: <br> triggerPrice > market price <br> Else, triggerPrice < market price <br> For spot, it is the TP/SL and Conditional order trigger price |
| triggerBy | false | string | Trigger price type, Conditional order param for Perps & Futures. <br> LastPrice <br> IndexPrice <br> MarkPrice <br> Valid for linear & inverse |
| orderIv | false | string | Implied volatility. option only. Pass the real value, e.g for 10%, 0.1 should be passed. orderIv has a higher priority when price is passed as well |
| timeinfo
rce | false | string | Time in force <br> Market order will always use IOC <br> If not passed, GTC is used by default |
| positionIdx | false | integer | Used to identify positions in different position modes. Under hedge-mode, this param is required <br> 0: one-way mode <br>1: hedge-mode Buy side <br> 2: hedge-mode Sell side |
| orderLinkId | false | string | User customised order ID. A max of 36 characters. Combinations of numbers, letters (upper and lower cases), dashes, and underscores are supported. <br> Futures & Perps: orderLinkId rules: <br> optional param <br> always unique <br> option orderLinkId rules: <br> required param <br> always unique |
| takeProfit | false | string | Take profit price <br> UTA: Spot Limit order supports take profit, stop loss or limit take profit, limit stop loss when creating an order |
| stopLoss | false | string | Stop loss price <br> UTA: Spot Limit order supports take profit, stop loss or limit take profit, limit stop loss when creating an order |
| tpTriggerBy | false | string | The price type to trigger take profit. MarkPrice, IndexPrice, default: LastPrice. Valid for linear & inverse |
| slTriggerBy | false | string | The price type to trigger stop loss. MarkPrice, IndexPrice, default: LastPrice. Valid for linear & inverse |
| reduceOnly | false | boolean | What is a reduce-only order? true means your position can only reduce in size if this order is triggered. <br> You must specify it as true when you are about to close/reduce the position <br> When reduceOnly is true, take profit/stop loss cannot be set <br> Valid for linear, inverse & option |
| closeOnTrigger | false | boolean | What is a close on trigger order? For a closing order. It can only reduce your position, not increase it. If the account has insufficient available balance when the closing order is triggered, then other active orders of similar contracts will be cancelled or reduced. It can be used to ensure your stop loss reduces your position regardless of current available margin. <br> Valid for linear & inverse |
| smpType | false | string | Smp execution type. What is SMP? |
| mmp | false | boolean | Market maker protection. option only. true means set the order as a market maker protection order. What is mmp? |
| tpslMode | false | string | TP/SL mode <br> Full: entire position for TP/SL. Then, tpOrderType or slOrderType must be Market <br> Partial: partial position tp/sl (as there is no size option, so it will create tp/sl orders with the qty you actually fill). Limit TP/SL order are supported. Note
: When create limit tp/sl, tpslMode is required and it must be Partial <br> Valid for linear & inverse |
| tpLimitPrice | false | string | The limit order price when take profit price is triggered <br> linear & inverse: only works when tpslMode=Partial and tpOrderType=Limit <br> Spot(UTA): it is required when the order has takeProfit and "tpOrderType"=Limit |
| slLimitPrice | false | string | The limit order price when stop loss price is triggered <br> linear & inverse: only works when tpslMode=Partial and slOrderType=Limit <br> Spot(UTA): it is required when the order has stopLoss and "slOrderType"=Limit |
| tpOrderType | false | string | The order type when take profit is triggered <br> linear & inverse: Market(default), Limit. For tpslMode=Full, it only supports tpOrderType=Market <br> Spot(UTA): <br> Market: when you set "takeProfit", <br> Limit: when you set "takeProfit" and "tpLimitPrice" |
| slOrderType | false | string | The order type when stop loss is triggered <br> linear & inverse: Market(default), Limit. For tpslMode=Full, it only supports slOrderType=Market<br> Spot(UTA): <br> Market: when you set "stopLoss", <br> Limit: when you set "stopLoss" and "slLimitPrice" |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| orderId | string | Order ID |
| orderLinkId | string | User customised order ID |

info
 

The acknowledgement of an place order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status. 

---



Request Example

HTTP
  
```http
POST /v5/order/create HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672211928338
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

// Spot Limit order with market tp sl
```json
{"category": "spot","symbol": "BTCUSDT","side": "Buy","orderType": "Limit","qty": "0.01","price": "28000","timeinfo
rce": "PostOnly","takeProfit": "35000","stopLoss": "27000","tpOrderType": "Market","slOrderType": "Market"}
```

// Spot Limit order with limit tp sl
```json
{"category": "spot","symbol": "BTCUSDT","side": "Buy","orderType": "Limit","qty": "0.01","price": "28000","timeinfo
rce": "PostOnly","takeProfit": "35000","stopLoss": "27000","tpLimitPrice": "36000","slLimitPrice": "27500","tpOrderType": "Limit","slOrderType": "Limit"}
```

// Spot PostOnly normal order
```json
{"category":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"0.1","price":"15600","timeinfo
rce":"PostOnly","orderLinkId":"spot-test-01","isLeverage":0,"orderFilter":"Order"}
```

// Spot TP/SL order
```json
{"category":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"0.1","price":"15600","triggerPrice": "15000", "timeinfo
rce":"Limit","orderLinkId":"spot-test-02","isLeverage":0,"orderFilter":"tpslOrder"}
```

// Spot margin normal order (UTA)
```json
{"category":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"0.1","price":"15600","timeinfo
rce":"GTC","orderLinkId":"spot-test-limit","isLeverage":1,"orderFilter":"Order"}
```

// Spot Market Buy order, qty is quote currency
```json
{"category":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Market","qty":"200","timeinfo
rce":"IOC","orderLinkId":"spot-test-04","isLeverage":0,"orderFilter":"Order"}
```


// USDT Perp open long position (one-way mode)
```json
{"category":"linear","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"1","price":"25000","timeinfo
rce":"GTC","positionIdx":0,"orderLinkId":"usdt-test-01","reduceOnly":false,"takeProfit":"28000","stopLoss":"20000","tpslMode":"Partial","tpOrderType":"Limit","slOrderType":"Limit","tpLimitPrice":"27500","slLimitPrice":"20500"}
```

// USDT Perp close long position (one-way mode)
```json
{"category": "linear", "symbol": "BTCUSDT", "side": "Sell", "orderType": "Limit", "qty": "1", "price": "30000", "timeinfo
rce": "GTC", "positionIdx": 0, "orderLinkId": "usdt-test-02", "reduceOnly": true}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderId": "1321003749386327552",
        "orderLinkId": "spot-test-postonly"
    },
    "retExtinfo
": {},
    "time": 1672211918471
}
```

