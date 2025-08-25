# Error Codes

> The error JSON payload:

```json
{  
  "code":-1121,  
  "msg":"Invalid symbol."  
}
```

Errors consist of two parts: an error code and a message. Codes are universal, but messages can vary.

## 10xx - General Server or Network issues​

### -1000 UNKNOWN​

* An unknown error occurred while processing the request.
* An unknown error occurred while processing the request.[%s]

### -1001 DISCONNECTED​

* Internal error; unable to process your request. Please try again.

### -1002 UNAUTHORIZED​

* You are not authorized to execute this request.

### -1003 TOO\_MANY\_REQUESTS​

* Too many requests queued.
* Too much request weight used; current limit is %s request weight per %s. Please use WebSocket Streams for live updates to avoid polling the API.
* Way too much request weight used; IP banned until %s. Please use WebSocket Streams for live updates to avoid bans.

### -1004 SERVER\_BUSY​

* Server is busy, please wait and try again

### -1006 UNEXPECTED\_RESP​

* An unexpected response was received from the message bus. Execution status unknown.

### -1007 TIMEOUT​

* Timeout waiting for response from backend server. Send status unknown; execution status unknown.

### -1008 SERVER\_BUSY​

* Spot server is currently overloaded with other requests. Please try again in a few minutes.

### -1014 UNKNOWN\_ORDER\_COMPOSITION​

* Unsupported order combination.

### -1015 TOO\_MANY\_ORDERS​

* Too many new orders.
* Too many new orders; current limit is %s orders per %s.

### -1016 SERVICE\_SHUTTING\_DOWN​

* This service is no longer available.

### -1020 UNSUPPORTED\_OPERATION​

* This operation is not supported.

### -1021 INVALID\_TIMESTAMP​

* Timestamp for this request is outside of the recvWindow.
* Timestamp for this request was 1000ms ahead of the server's time.

### -1022 INVALID\_SIGNATURE​

* Signature for this request is not valid.

### -1099 Not found, authenticated, or authorized​

* This replaces error code -1999

## 11xx - 2xxx Request issues​

### -1100 ILLEGAL\_CHARS​

* Illegal characters found in a parameter.
* Illegal characters found in a parameter. %s
* Illegal characters found in parameter `%s`; legal range is `%s`.

### -1101 TOO\_MANY\_PARAMETERS​

* Too many parameters sent for this endpoint.
* Too many parameters; expected `%s` and received `%s`.
* Duplicate values for a parameter detected.

### -1102 MANDATORY\_PARAM\_EMPTY\_OR\_MALFORMED​

* A mandatory parameter was not sent, was empty/null, or malformed.
* Mandatory parameter `%s` was not sent, was empty/null, or malformed.
* Param `%s` or `%s` must be sent, but both were empty/null!

### -1103 UNKNOWN\_PARAM​

* An unknown parameter was sent.

### -1104 UNREAD\_PARAMETERS​

* Not all sent parameters were read.
* Not all sent parameters were read; read `%s` parameter(s) but was sent `%s`.

### -1105 PARAM\_EMPTY​

* A parameter was empty.
* Parameter `%s` was empty.

### -1106 PARAM\_NOT\_REQUIRED​

* A parameter was sent when not required.
* Parameter `%s` sent when not required.

### -1111 BAD\_PRECISION​

* Precision is over the maximum defined for this asset.

### -1112 NO\_DEPTH​

* No orders on book for symbol.

### -1114 TIF\_NOT\_REQUIRED​

* TimeInForce parameter sent when not required.

### -1115 INVALID\_TIF​

* Invalid timeInForce.

### -1116 INVALID\_ORDER\_TYPE​

* Invalid orderType.

### -1117 INVALID\_SIDE​

* Invalid side.

### -1118 EMPTY\_NEW\_CL\_ORD\_ID​

* New client order ID was empty.

### -1119 EMPTY\_ORG\_CL\_ORD\_ID​

* Original client order ID was empty.

### -1120 BAD\_INTERVAL​

* Invalid interval.

### -1121 BAD\_SYMBOL​

* Invalid symbol.

### -1125 INVALID\_LISTEN\_KEY​

* This listenKey does not exist.

### -1127 MORE\_THAN\_XX\_HOURS​

* Lookup interval is too big.
* More than %s hours between startTime and endTime.

### -1128 OPTIONAL\_PARAMS\_BAD\_COMBO​

* Combination of optional parameters invalid.

### -1130 INVALID\_PARAMETER​

* Invalid data sent for a parameter.
* Data sent for parameter `%s` is not valid.

### -1131 BAD\_RECV\_WINDOW​

* recvWindow must be less than 60000

### -1134 BAD\_STRATEGY\_TYPE​

* `strategyType` was less than 1000000.

#### -1145 INVALID\_CANCEL\_RESTRICTIONS​

* `cancelRestrictions` has to be either `ONLY_NEW` or `ONLY_PARTIALLY_FILLED`.

#### -1151 DUPLICATE\_SYMBOLS​

* Symbol is present multiple times in the list.

### -2010 NEW\_ORDER\_REJECTED​

* NEW\_ORDER\_REJECTED

### -2011 CANCEL\_REJECTED​

* CANCEL\_REJECTED

### -2013 NO\_SUCH\_ORDER​

* Order does not exist.

### -2014 BAD\_API\_KEY\_FMT​

* API-key format invalid.

### -2015 REJECTED\_MBX\_KEY​

* Invalid API-key, IP, or permissions for action.

### -2016 NO\_TRADING\_WINDOW​

* No trading window could be found for the symbol. Try ticker/24hrs instead.

#### -2026 ORDER\_ARCHIVED​

* Order was canceled or expired with no executed qty over 90 days ago and has been archived.

## 3xxx-5xxx SAPI-specific issues​

### -3000 INNER\_FAILURE​

* Internal server error.

### -3001 NEED\_ENABLE\_2FA​

* Please enable 2FA first.

### -3002 ASSET\_DEFICIENCY​

* We don't have this asset.

### -3003 NO\_OPENED\_MARGIN\_ACCOUNT​

* Margin account does not exist.

### -3004 TRADE\_NOT\_ALLOWED​

* Trade not allowed.

### -3005 TRANSFER\_OUT\_NOT\_ALLOWED​

* Transferring out not allowed.

### -3006 EXCEED\_MAX\_BORROWABLE​

* Your borrow amount has exceed maximum borrow amount.

### -3007 HAS\_PENDING\_TRANSACTION​

* You have pending transaction, please try again later.

### -3008 BORROW\_NOT\_ALLOWED​

* Borrow not allowed.

### -3009 ASSET\_NOT\_MORTGAGEABLE​

* This asset are not allowed to transfer into margin account currently.

### -3010 REPAY\_NOT\_ALLOWED​

* Repay not allowed.

### -3011 BAD\_DATE\_RANGE​

* Your input date is invalid.

### -3012 ASSET\_ADMIN\_BAN\_BORROW​

* Borrow is banned for this asset.

### -3013 LT\_MIN\_BORROWABLE​

* Borrow amount less than minimum borrow amount.

### -3014 ACCOUNT\_BAN\_BORROW​

* Borrow is banned for this account.

### -3015 REPAY\_EXCEED\_LIABILITY​

* Repay amount exceeds borrow amount.

### -3016 LT\_MIN\_REPAY​

* Repay amount less than minimum repay amount.

### -3017 ASSET\_ADMIN\_BAN\_MORTGAGE​

* This asset are not allowed to transfer into margin account currently.

### -3018 ACCOUNT\_BAN\_MORTGAGE​

* Transferring in has been banned for this account.

### -3019 ACCOUNT\_BAN\_ROLLOUT​

* Transferring out has been banned for this account.

### -3020 EXCEED\_MAX\_ROLLOUT​

* Transfer out amount exceeds max amount.

### -3021 PAIR\_ADMIN\_BAN\_TRADE​

* Margin account are not allowed to trade this trading pair.

### -3022 ACCOUNT\_BAN\_TRADE​

* You account's trading is banned.

### -3023 WARNING\_MARGIN\_LEVEL​

* You can't transfer out/place order under current margin level.

### -3024 FEW\_LIABILITY\_LEFT​

* The unpaid debt is too small after this repayment.

### -3025 INVALID\_EFFECTIVE\_TIME​

* Your input date is invalid.

### -3026 VALIDATION\_FAILED​

* Your input param is invalid.

### -3027 NOT\_VALID\_MARGIN\_ASSET​

* Not a valid margin asset.

### -3028 NOT\_VALID\_MARGIN\_PAIR​

* Not a valid margin pair.

### -3029 TRANSFER\_FAILED​

* Transfer failed.

### -3036 ACCOUNT\_BAN\_REPAY​

* This account is not allowed to repay.

### -3037 PNL\_CLEARING​

* PNL is clearing. Wait a second.

### -3038 LISTEN\_KEY\_NOT\_FOUND​

* Listen key not found.

### -3041 BALANCE\_NOT\_CLEARED​

* Balance is not enough

### -3042 PRICE\_INDEX\_NOT\_FOUND​

* PriceIndex not available for this margin pair.

### -3043 TRANSFER\_IN\_NOT\_ALLOWED​

* Transferring in not allowed.

### -3044 SYSTEM\_BUSY​

* System busy.

### -3045 SYSTEM​

* The system doesn't have enough asset now.

### -3999 NOT\_WHITELIST\_USER​

* This function is only available for invited users.

### -4001 CAPITAL\_INVALID​

* Invalid operation.

### -4002 CAPITAL\_IG​

* Invalid get.

### -4003 CAPITAL\_IEV​

* Your input email is invalid.

### -4004 CAPITAL\_UA​

* You don't login or auth.

### -4005 CAPAITAL\_TOO\_MANY\_REQUEST​

* Too many new requests.

### -4006 CAPITAL\_ONLY\_SUPPORT\_PRIMARY\_ACCOUNT​

* Support main account only.

### -4007 CAPITAL\_ADDRESS\_VERIFICATION\_NOT\_PASS​

* Address validation is not passed.

### -4008 CAPITAL\_ADDRESS\_TAG\_VERIFICATION\_NOT\_PASS​

* Address tag validation is not passed.

### -4010 CAPITAL\_WHITELIST\_EMAIL\_CONFIRM​

* White list mail has been confirmed.

### -4011 CAPITAL\_WHITELIST\_EMAIL\_EXPIRED​

* White list mail is invalid.

### -4012 CAPITAL\_WHITELIST\_CLOSE​

* White list is not opened.

### -4013 CAPITAL\_WITHDRAW\_2FA\_VERIFY​

* 2FA is not opened.

### -4014 CAPITAL\_WITHDRAW\_LOGIN\_DELAY​

* Withdraw is not allowed within 2 min login.

### -4015 CAPITAL\_WITHDRAW\_RESTRICTED\_MINUTE​

* Withdraw is limited.

### -4016 CAPITAL\_WITHDRAW\_RESTRICTED\_PASSWORD​

* Within 24 hours after password modification, withdrawal is prohibited.

### -4017 CAPITAL\_WITHDRAW\_RESTRICTED\_UNBIND\_2FA​

* Within 24 hours after the release of 2FA, withdrawal is prohibited.

### -4018 CAPITAL\_WITHDRAW\_ASSET\_NOT\_EXIST​

* We don't have this asset.

### -4019 CAPITAL\_WITHDRAW\_ASSET\_PROHIBIT​

* Current asset is not open for withdrawal.

### -4021 CAPITAL\_WITHDRAW\_AMOUNT\_MULTIPLE​

* Asset withdrawal must be an %s multiple of %s.

### -4022 CAPITAL\_WITHDRAW\_MIN\_AMOUNT​

* Not less than the minimum pick-up quantity %s.

### -4023 CAPITAL\_WITHDRAW\_MAX\_AMOUNT​

* Within 24 hours, the withdrawal exceeds the maximum amount.

### -4024 CAPITAL\_WITHDRAW\_USER\_NO\_ASSET​

* You don't have this asset.

### -4025 CAPITAL\_WITHDRAW\_USER\_ASSET\_LESS\_THAN\_ZERO​

* The number of hold asset is less than zero.

### -4026 CAPITAL\_WITHDRAW\_USER\_ASSET\_NOT\_ENOUGH​

* You have insufficient balance.

### -4027 CAPITAL\_WITHDRAW\_GET\_TRAN\_ID\_FAILURE​

* Failed to obtain tranId.

### -4028 CAPITAL\_WITHDRAW\_MORE\_THAN\_FEE​

* The amount of withdrawal must be greater than the Commission.

### -4029 CAPITAL\_WITHDRAW\_NOT\_EXIST​

* The withdrawal record does not exist.

### -4030 CAPITAL\_WITHDRAW\_CONFIRM\_SUCCESS​

* Confirmation of successful asset withdrawal.

### -4031 CAPITAL\_WITHDRAW\_CANCEL\_FAILURE​

* Cancellation failed.

### -4032 CAPITAL\_WITHDRAW\_CHECKSUM\_VERIFY\_FAILURE​

* Withdraw verification exception.

### -4033 CAPITAL\_WITHDRAW\_ILLEGAL\_ADDRESS​

* Illegal address.

### -4034 CAPITAL\_WITHDRAW\_ADDRESS\_CHEAT​

* The address is suspected of fake.

### -4035 CAPITAL\_WITHDRAW\_NOT\_WHITE\_ADDRESS​

* This address is not on the whitelist. Please join and try again.

### -4036 CAPITAL\_WITHDRAW\_NEW\_ADDRESS​

* The new address needs to be withdrawn in {0} hours.

### -4037 CAPITAL\_WITHDRAW\_RESEND\_EMAIL\_FAIL​

* Re-sending Mail failed.

### -4038 CAPITAL\_WITHDRAW\_RESEND\_EMAIL\_TIME\_OUT​

* Please try again in 5 minutes.

### -4039 CAPITAL\_USER\_EMPTY​

* The user does not exist.

### -4040 CAPITAL\_NO\_CHARGE​

* This address not charged.

### -4041 CAPITAL\_MINUTE\_TOO\_SMALL​

* Please try again in one minute.

### -4042 CAPITAL\_CHARGE\_NOT\_RESET​

* This asset cannot get deposit address again.

### -4043 CAPITAL\_ADDRESS\_TOO\_MUCH​

* More than 100 recharge addresses were used in 24 hours.

### -4044 CAPITAL\_BLACKLIST\_COUNTRY\_GET\_ADDRESS​

* This is a blacklist country.

### -4045 CAPITAL\_GET\_ASSET\_ERROR​

* Failure to acquire assets.

### -4046 CAPITAL\_AGREEMENT\_NOT\_CONFIRMED​

* Agreement not confirmed.

### -4047 CAPITAL\_DATE\_INTERVAL\_LIMIT​

* Time interval must be within 0-90 days

### -4060 CAPITAL\_WITHDRAW\_USER\_ASSET\_LOCK\_DEPOSIT​

* As your deposit has not reached the required block confirmations, we have temporarily locked {0} asset

### -5001 ASSET\_DRIBBLET\_CONVERT\_SWITCH\_OFF​

* Don't allow transfer to micro assets.

### -5002 ASSET\_ASSET\_NOT\_ENOUGH​

* You have insufficient balance.

### -5003 ASSET\_USER\_HAVE\_NO\_ASSET​

* You don't have this asset.

### -5004 USER\_OUT\_OF\_TRANSFER\_FLOAT​

* The residual balances have exceeded 0.001BTC, Please re-choose.
* The residual balances of %s have exceeded 0.001BTC, Please re-choose.

### -5005 USER\_ASSET\_AMOUNT\_IS\_TOO\_LOW​

* The residual balances of the BTC is too low
* The residual balances of %s is too low, Please re-choose.

### -5006 USER\_CAN\_NOT\_REQUEST\_IN\_24\_HOURS​

* Only transfer once in 24 hours.

### -5007 AMOUNT\_OVER\_ZERO​

* Quantity must be greater than zero.

### -5008 ASSET\_WITHDRAW\_WITHDRAWING\_NOT\_ENOUGH​

* Insufficient amount of returnable assets.

### -5009 PRODUCT\_NOT\_EXIST​

* Product does not exist.

### -5010 TRANSFER\_FAIL​

* Asset transfer fail.

### -5011 FUTURE\_ACCT\_NOT\_EXIST​

* future account not exists.

### -5012 TRANSFER\_PENDING​

* Asset transfer is in pending.

### -5021 PARENT\_SUB\_HAVE\_NO\_RELATION​

* This parent sub have no relation

### -5012 FUTURE\_ACCT\_OR\_SUBRELATION\_NOT\_EXIST​

* future account or sub relation not exists.

## 6XXX - Savings Issues​

### -6001 DAILY\_PRODUCT\_NOT\_EXIST​

* Daily product not exists.

### -6003 DAILY\_PRODUCT\_NOT\_ACCESSIBLE​

* Product not exist or you don't have permission

### -6004 DAILY\_PRODUCT\_NOT\_PURCHASABLE​

* Product not in purchase status

### -6005 DAILY\_LOWER\_THAN\_MIN\_PURCHASE\_LIMIT​

* Smaller than min purchase limit

### -6006 DAILY\_REDEEM\_AMOUNT\_ERROR​

* Redeem amount error

### -6007 DAILY\_REDEEM\_TIME\_ERROR​

* Not in redeem time

### -6008 DAILY\_PRODUCT\_NOT\_REDEEMABLE​

* Product not in redeem status

### -6009 REQUEST\_FREQUENCY\_TOO\_HIGH​

* Request frequency too high

### -6011 EXCEEDED\_USER\_PURCHASE\_LIMIT​

* Exceeding the maximum num allowed to purchase per user

### -6012 BALANCE\_NOT\_ENOUGH​

* Balance not enough

### -6013 PURCHASING\_FAILED​

* Purchasing failed

### -6014 UPDATE\_FAILED​

* Exceed up-limit allowed to purchased

### -6015 EMPTY\_REQUEST\_BODY​

* Empty request body

### -6016 PARAMS\_ERR​

* Parameter err

### -6017 NOT\_IN\_WHITELIST​

* Not in whitelist

### -6018 ASSET\_NOT\_ENOUGH​

* Asset not enough

### -6019 PENDING​

* Need confirm

### -6020 PROJECT\_NOT\_EXISTS​

* Project not exists

## 70xx - Futures​

### -7001 FUTURES\_BAD\_DATE\_RANGE​

* Date range is not supported.

### -7002 FUTURES\_BAD\_TYPE​

* Data request type is not supported.

## 20xxx - Futures/Spot Algo​

### -20121​

* Invalid symbol.

### -20124​

* Invalid algo id or it has been completed.

### -20130​

* Invalid data sent for a parameter.

### -20132​

* The client algo id is duplicated.

### -20194​

* Duration is too short to execute all required quantity.

### -20195​

* The total size is too small.

### -20196​

* The total size is too large.

### -20198​

* Reach the max open orders allowed.

### -20204​

* The notional of USD is less or more than the limit.

## Filter failures​

| Error message | Description |
| --- | --- |
| "Filter failure: PRICE\_FILTER" | `price` is too high, too low, and/or not following the tick size rule for the symbol. |
| "Filter failure: PERCENT\_PRICE" | `price` is X% too high or X% too low from the average weighted price over the last Y minutes. |
| "Filter failure: PERCENT\_PRICE\_BY\_SIDE" | `price` is X% too high or Y% too low from the `lastPrice` on that side (i.e. BUY/SELL) |
| "Filter failure: LOT\_SIZE" | `quantity` is too high, too low, and/or not following the step size rule for the symbol. |
| "Filter failure: MIN\_NOTIONAL" | `price` \* `quantity` is too low to be a valid order for the symbol. |
| "Filter failure: ICEBERG\_PARTS" | `ICEBERG` order would break into too many parts; icebergQty is too small. |
| "Filter failure: MARKET\_LOT\_SIZE" | `MARKET` order's `quantity` is too high, too low, and/or not following the step size rule for the symbol. |
| "Filter failure: MAX\_POSITION" | The account's position has reached the maximum defined limit.    This is composed of the sum of the balance of the base asset, and the sum of the quantity of all open `BUY`orders. |
| "Filter failure: MAX\_NUM\_ORDERS" | Account has too many open orders on the symbol. |
| "Filter failure: MAX\_NUM\_ALGO\_ORDERS" | Account has too many open stop loss and/or take profit orders on the symbol. |
| "Filter failure: MAX\_NUM\_ICEBERG\_ORDERS" | Account has too many open iceberg orders on the symbol. |
| "Filter failure: TRAILING\_DELTA" | `trailingDelta` is not within the defined range of the filter for that order type. |
| "Filter failure: EXCHANGE\_MAX\_NUM\_ORDERS" | Account has too many open orders on the exchange. |
| "Filter failure: EXCHANGE\_MAX\_NUM\_ALGO\_ORDERS" | Account has too many open stop loss and/or take profit orders on the exchange. |

## 10xxx - Crypto Loans​

### -10001 SYSTEM\_MAINTENANCE​

* The system is under maintenance, please try again later.

### -10002 INVALID\_INPUT​

* Invalid input parameters.

### -10005 NO\_RECORDS​

* No records found.

### -10007 COIN\_NOT\_LOANABLE​

* This coin is not loanable.

### -10008 COIN\_NOT\_LOANABLE​

* This coin is not loanable

### -10009 COIN\_NOT\_COLLATERAL​

* This coin can not be used as collateral.

### -10010 COIN\_NOT\_COLLATERAL​

* This coin can not be used as collateral.

### -10011 INSUFFICIENT\_ASSET​

* Insufficient spot assets.

### -10012 INVALID\_AMOUNT​

* Invalid repayment amount.

### -10013 INSUFFICIENT\_AMOUNT​

* Insufficient collateral amount.

### -10015 DEDUCTION\_FAILED​

* Collateral deduction failed.

### -10016 LOAN\_FAILED​

* Failed to provide loan.

### -10017 REPAY\_EXCEED\_DEBT​

* Repayment amount exceeds debt.

### -10018 INVALID\_AMOUNT​

* Invalid repayment amount.

### -10019 CONFIG\_NOT\_EXIST​

* Configuration does not exists.

### -10020 UID\_NOT\_EXIST​

* User ID does not exist.

### -10021 ORDER\_NOT\_EXIST​

* Order does not exist.

### -10022 INVALID\_AMOUNT​

* Invalid adjustment amount.

### -10023 ADJUST\_LTV\_FAILED​

* Failed to adjust LTV.

### -10024 ADJUST\_LTV\_NOT\_SUPPORTED​

* LTV adjustment not supported.

### -10025 REPAY\_FAILED​

* Repayment failed.

### -10026 INVALID\_PARAMETER​

* Invalid parameter.

### -10028 INVALID\_PARAMETER​

* Invalid parameter.

### -10029 AMOUNT\_TOO\_SMALL​

* Loan amount is too small.

### -10030 AMOUNT\_TOO\_LARGE​

* Loan amount is too much.

### -10031 QUOTA\_REACHED​

* Individual loan quota reached.

### -10032 REPAY\_NOT\_AVAILABLE​

* Repayment is temporarily unavailable.

### -10034 REPAY\_NOT\_AVAILABLE​

* Repay with collateral is not available currently, please try to repay with borrowed coin.

### -10039 AMOUNT\_TOO\_SMALL​

* Repayment amount is too small.

### -10040 AMOUNT\_TOO\_LARGE​

* Repayment amount is too large.

### -10041 INSUFFICIENT\_AMOUNT​

* Due to high demand, there are currently insufficient loanable assets for {0}. Please adjust your borrow amount or try again tomorrow.

### -10042 ASSET\_NOT\_SUPPORTED​

* asset %s is not supported

### -10043 ASSET\_NOT\_SUPPORTED​

* {0} borrowing is currently not supported.

### -10044 QUOTA\_REACHED​

* Collateral amount has reached the limit. Please reduce your collateral amount or try with other collaterals.

### -10045 COLLTERAL\_REPAY\_NOT\_SUPPORTED​

* The loan coin does not support collateral repayment. Please try again later.

### -10046 EXCEED\_MAX\_ADJUSTMENT​

* Collateral Adjustment exceeds the maximum limit. Please try again.

### -10047 REGION\_NOT\_SUPPORTED​

* This coin is currently not supported in your location due to local regulations.

## 13xxx - BLVT​

### -13000 BLVT\_FORBID\_REDEEM​

* Redeption of the token is forbiden now

### -13001 BLVT\_EXCEED\_DAILY\_LIMIT​

* Exceeds individual 24h redemption limit of the token

### -13002 BLVT\_EXCEED\_TOKEN\_DAILY\_LIMIT​

* Exceeds total 24h redemption limit of the token

### -13003 BLVT\_FORBID\_PURCHASE​

* Subscription of the token is forbiden now

### -13004 BLVT\_EXCEED\_DAILY\_PURCHASE\_LIMIT​

* Exceeds individual 24h subscription limit of the token

### -13005 BLVT\_EXCEED\_TOKEN\_DAILY\_PURCHASE\_LIMIT​

* Exceeds total 24h subscription limit of the token

### -13006 BLVT\_PURCHASE\_LESS\_MIN\_AMOUNT​

* Subscription amount is too small

### -13007 BLVT\_PURCHASE\_AGREEMENT\_NOT\_SIGN​

* The Agreement is not signed

## 12xxx - Liquid Swap​

### -12014 TOO MANY REQUESTS​

* More than 1 request in 2 seconds

## 18xxx - Binance Code​

### -18002​

* The total amount of codes you created has exceeded the 24-hour limit, please try again after UTC 0

### -18003​

* Too many codes created in 24 hours, please try again after UTC 0

### -18004​

* Too many invalid redeem attempts in 24 hours, please try again after UTC 0

### -18005​

* Too many invalid verify attempts, please try later

### -18006​

* The amount is too small, please re-enter

### -18007​

* This token is not currently supported, please re-enter

## 21xxx - Portfolio Margin Account​

### -21001 USER\_IS\_NOT\_UNIACCOUNT​

* Request ID is not a Portfolio Margin Account.

### -21002 UNI\_ACCOUNT\_CANT\_TRANSFER\_FUTURE​

* Portfolio Margin Account doesn't support transfer from margin to futures.

### -21003 NET\_ASSET\_MUST\_LTE\_RATIO​

* Fail to retrieve margin assets.

### -21004 USER\_NO\_LIABILITY​

* User doesn’t have portfolio margin bankruptcy loan

### -21005 NO\_ENOUGH\_ASSET​

* User’s spot wallet doesn’t have enough BUSD to repay portfolio margin bankruptcy loan

### -21006 HAD\_IN\_PROCESS\_REPAY​

* User had portfolio margin bankruptcy loan repayment in process

### -21007 IN\_FORCE\_LIQUIDATION​

* User failed to repay portfolio margin bankruptcy loan since liquidation was in process

## Order Rejection Issues​

Error messages like these are indicated when the error is coming specifically from the matching engine:

* `-1010 ERROR_MSG_RECEIVED`
* `-2010 NEW_ORDER_REJECTED`
* `-2011 CANCEL_REJECTED`

The following messages which will indicate the specific error:

| Error message | Description |
| --- | --- |
| "Unknown order sent." | The order (by either `orderId`, `clientOrderId`, `origClientOrderId`) could not be found. |
| "Duplicate order sent." | The `clientOrderId` is already in use. |
| "Market is closed." | The symbol is not trading. |
| "Account has insufficient balance for requested action." | Not enough funds to complete the action. |
| "Market orders are not supported for this symbol." | `MARKET` is not enabled on the symbol. |
| "Iceberg orders are not supported for this symbol." | `icebergQty` is not enabled on the symbol |
| "Stop loss orders are not supported for this symbol." | `STOP_LOSS` is not enabled on the symbol |
| "Stop loss limit orders are not supported for this symbol." | `STOP_LOSS_LIMIT` is not enabled on the symbol |
| "Take profit orders are not supported for this symbol." | `TAKE_PROFIT` is not enabled on the symbol |
| "Take profit limit orders are not supported for this symbol." | `TAKE_PROFIT_LIMIT` is not enabled on the symbol |
| "Price \* QTY is zero or less." | `price` \* `quantity` is too low |
| "IcebergQty exceeds QTY." | `icebergQty` must be less than the order quantity |
| "This action is disabled on this account." | Contact customer support; some actions have been disabled on the account. |
| "This account may not place or cancel orders." | Contact customer support; the account has trading ability disabled. |
| "Unsupported order combination" | The `orderType`, `timeInForce`, `stopPrice`, and/or `icebergQty` combination isn't allowed. |
| "Order would trigger immediately." | The order's stop price is not valid when compared to the last traded price. |
| "Cancel order is invalid. Check origClientOrderId and orderId." | No `origClientOrderId` or `orderId` was sent in. |
| "Order would immediately match and take." | `LIMIT_MAKER` order type would immediately match and trade, and not be a pure maker order. |
| "The relationship of the prices for the orders is not correct." | The prices set in the `OCO` is breaking the Price rules.    The rules are:    `SELL Orders`: Limit Price > Last Price > Stop Price   `BUY Orders`: Limit Price < Last Price < Stop Price |
| "OCO orders are not supported for this symbol" | `OCO` is not enabled on the symbol. |
| "Quote order qty market orders are not support for this symbol." | `MARKET` orders using the parameter `quoteOrderQty` are not enabled on this symbol. |
| "Trailing stop orders are not supported for this symbol." | Orders using `trailingDelta` are not enabled on the symbol. |
| "Order cancel-replace is not supported for this symbol." | `POST /api/v3/order/cancelReplace` (REST API) or `order.cancelReplace` (WebSocket API) is on enabled the symbol. |
| "This symbol is not permitted for this account." | Account and symbol do not have the same permissions. (e.g. `SPOT`, `MARGIN`, etc) |
| "This symbol is restricted for this account." | Account is unable to trade on that symbol. (e.g. An `ISOLATED_MARGIN` account cannot place `SPOT` orders.) |
| "Order was not canceled due to cancel restrictions." | Either `cancelRestrictions` was set to `ONLY_NEW` but the order status was not `NEW`   or   `cancelRestrictions` was set to `ONLY_PARTIALLY_FILLED` but the order status was not `PARTIALLY_FILLED`. |

## Errors regarding POST /api/v3/order/cancelReplace​

### -2021 Order cancel-replace partially failed​

This code is sent when either the cancellation of the order failed or the new order placement failed but not both.

### -2022 Order cancel-replace failed.​

This code is sent when both the cancellation of the order failed and the new order placement failed.

