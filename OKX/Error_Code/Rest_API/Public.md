### Public
Error Code from 50000 to 53999

#### General Class

| Error Code | HTTP Status Code | Error Message |
|------------|------------------|---------------|
| 0 | 200 | |
| 1 | 200 | Operation failed. |
| 2 | 200 | Bulk operation partially succeeded. |
| 50000 | 400 | Body for POST request cannot be empty. |
| 50001 | 503 | Service temporarily unavailable. Please try again later. |
| 50002 | 400 | JSON syntax error |
| 50004 | 400 | API endpoint request timeout. (does not mean that the request was successful or failed, please check the request result). |
| 50005 | 410 | API endpoint is inactive or unavailable. |
| 50006 | 400 | Invalid Content-Type. Please use "application/JSON". |
| 50007 | 200 | User blocked. |
| 50008 | 200 | User doesn't exist. |
| 50009 | 200 | Account is frozen due to stop-out. |
| 50010 | 200 | User ID cannot be empty. |
| 50011 | 200 | Rate limit reached. Please refer to API documentation and throttle requests accordingly. |
| 50011 | 429 | Too Many Requests |
| 50012 | 200 | Account status invalid. Check account status |
| 50013 | 429 | Systems are busy. Please try again later. |
| 50014 | 400 | Parameter {param0} can not be empty. |
| 50015 | 400 | Either parameter {param0} or {param1} is required |
| 50016 | 400 | Parameter {param0} does not match parameter {param1} |
| 50017 | 200 | Position frozen and related operations restricted due to auto-deleveraging (ADL). Please try again later. |
| 50018 | 200 | Currency {param0} is frozen due to ADL. Operation restricted. |
| 50019 | 200 | Account frozen and related operations restricted due to auto-deleveraging (ADL). Please try again later. |
| 50020 | 200 | Position frozen and related operations restricted due to forced liquidation. Please try again later. |
| 50021 | 200 | Currency {param0} is frozen due to liquidation. Operation restricted. |
| 50022 | 200 | Account frozen and related operations restricted due to forced liquidation. Please try again later. |
| 50023 | 200 | Funding fees frozen and related operations are restricted. Please try again later. |
| 50024 | 200 | Parameter {param0} and {param1} can not exist at the same time. |
| 50025 | 200 | Parameter {param0} count exceeds the limit {param1}. |
| 50026 | 500 | System error. Try again later |
| 50027 | 200 | This account is restricted from trading. Please contact customer support for assistance. |
| 50028 | 200 | Unable to place the order. Please contact the customer service for details. |
| 50029 | 200 | Your account has triggered OKX risk control and is temporarily restricted from conducting transactions. Please check your email registered with OKX for contact from our customer support team. |
| 50030 | 200 | You don't have permission to use this API endpoint |
| 50032 | 200 | Your account has been set to prohibit transactions in this currency. Please confirm and try again |
| 50033 | 200 | Instrument blocked. Please verify trading this instrument is allowed under account settings and try again. |
| 50035 | 403 | This endpoint requires that APIKey must be bound to IP |
| 50036 | 200 | The expTime can't be earlier than the current system time. Please adjust the expTime and try again. |
| 50037 | 200 | Order expired. |
| 50038 | 200 | This feature is unavailable in demo trading |
| 50039 | 200 | Parameter "before" isn't supported for timestamp pagination |
| 50040 | 200 | Too frequent operations, please try again later |
| 50041 | 200 | Your user ID hasn't been allowlisted. Please contact customer service for assistance. |
| 50042 | 200 | Repeated request |
| 50044 | 200 | Must select one broker type |
| 50045 | 200 | simPos should be empty because simulated positions cannot be counted when Position Builder is calculating under Spot-Derivatives risk offset mode |
| 50046 | 200 | This feature is temporarily unavailable while we make some improvements to it. Please try again later. |
| 50047 | 200 | {param0} has already settled. To check the relevant candlestick data, please use {param1} |
| 50048 | 200 | Switching risk unit may lead position risk increases and be forced liquidated. Please adjust position size, make sure margin is in a safe status. |
| 50049 | 200 | No information on the position tier. The current instrument doesn't support margin trading. |
| 50050 | 200 | You've already activated options trading. Please don't activate it again. |
| 50051 | 200 | Due to compliance restrictions in your country or region, you cannot use this feature. |
| 50052 | 200 | Due to local laws and regulations, you cannot trade with your chosen crypto. |
| 50053 | 200 | This feature is only available in demo trading. |
| 50055 | 200 | Reset unsuccessful. Assets can only be reset up to 5 times per day. |
| 50056 | 200 | You have pending orders or open positions with this currency. Please reset after canceling all the pending orders/closing all the open positions. |
| 50057 | 200 | Reset unsuccessful. Try again later. |
| 50058 | 200 | This crypto is not supported in an asset reset. |
| 50059 | 200 | Before you continue, you'll need to complete additional steps as required by your local regulators. Please visit the website or app for more details. |
| 50060 | 200 | For security and compliance purposes, please complete the identity verification process to continue using our services. |
| 50061 | 200 | You've reached the maximum order rate limit for this account. |
| 50062 | 200 | This feature is currently unavailable. |
| 50063 | 200 | You can't activate the credits as they might have expired or are already activated. |
| 50064 | 200 | The borrowing system is unavailable. Try again later. |
| 50067 | 200 | The API doesn't support cross site trading feature |
| 50069 | 200 | Margin ratio verification for this risk unit failed. |
| 50071 | 200 | {param} already exists<br/>e.g. clOrdId already exists |

#### API Class

| Error Code | HTTP Status Code | Error Message |
|------------|------------------|---------------|
| 50100 | 400 | API frozen, please contact customer service. |
| 50101 | 401 | APIKey does not match current environment. |
| 50102 | 401 | Timestamp request expired. |
| 50103 | 401 | Request header "OK-ACCESS-KEY" cannot be empty. |
| 50104 | 401 | Request header "OK-ACCESS-PASSPHRASE" cannot be empty. |
| 50105 | 401 | Request header "OK-ACCESS-PASSPHRASE" incorrect. |
| 50106 | 401 | Request header "OK-ACCESS-SIGN" cannot be empty. |
| 50107 | 401 | Request header "OK-ACCESS-TIMESTAMP" cannot be empty. |
| 50108 | 401 | Exchange ID does not exist. |
| 50109 | 401 | Exchange domain does not exist. |
| 50110 | 401 | Your IP {param0} is not included in your API key's IP whitelist. |
| 50111 | 401 | Invalid OK-ACCESS-KEY. |
| 50112 | 401 | Invalid OK-ACCESS-TIMESTAMP. |
| 50113 | 401 | Invalid signature. |
| 50114 | 401 | Invalid authorization. |
| 50115 | 405 | Invalid request method. |
| 50116 | 200 | Fast API is allowed to create only one API key |
| 50118 | 200 | To link the app using your API key, your broker needs to share their IP to be whitelisted |
| 50119 | 200 | API key doesn't exist |
| 50120 | 200 | This API key doesn't have permission to use this function |
| 50121 | 200 | You can't access our services through the IP address ({param0}) |
| 50122 | 200 | Order amount must exceed minimum amount |

#### Trade Class

| Error Code | HTTP Status Code | Error Message |
|------------|------------------|---------------|
| 51000 | 400 | Parameter {param0} error |
| 51001 | 200 | Instrument ID or Spread ID doesn't exist. |
| 51002 | 200 | Instrument ID doesn't match underlying index. |
| 51003 | 200 | Either client order ID or order ID is required. |
| 51004 | 200 | Order failed. For isolated long/short mode of {param0}, the sum of current order size, position quantity in the same direction, and pending orders in the same direction can't be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, current order size: {param3} contracts, position quantity in the same direction: {param4} contracts, pending orders in the same direction: {param5} contracts). |
| 51004 | 200 | Order failed. For cross long/short mode of {param0}, the sum of current order size, position quantity in the long and short directions, and pending orders in the long and short directions can't be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, current order size: {param3} contracts, position quantity in the long and short directions: {param4} contracts, pending orders in the long and short directions: {param5} contracts). |
| 51004 | 200 | Order failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of current order size, current instId position quantity in the long and short directions, current instId pending orders in the long and short directions, and other contracts of the same instFamily can't be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, current order size: {param4} contracts, current instId position quantity in the long and short directions: {param5} contracts, current instId pending orders in the long and short directions: {param6} contracts, other contracts of the same instFamily: {param7} contracts). |
| 51004 | 200 | Order failed. For buy/sell mode of {param0}, the sum of current buy order size, position quantity, and pending buy orders can't be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, current buy order size: {param3} contracts, position quantity: {param4} contracts, pending buy orders: {param5} contracts). |
| 51004 | 200 | Order failed. For buy/sell mode of {param0}, the sum of current sell order size, position quantity, and pending sell orders can't be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, current sell order size: {param3} contracts, position quantity: {param4} contracts, pending sell orders: {param5} contracts). |
| 51004 | 200 | Order failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of current buy order size, current instId position quantity, current instId pending buy orders, and other contracts of the same instFamily can't be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, current buy order size: {param4} contracts, current instId position quantity: {param5} contracts, current instId pending buy orders: {param6} contracts, other contracts of the same instFamily: {param7} contracts). |
| 51004 | 200 | Order failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of current sell order size, current instId position quantity, current instId pending sell orders, and other contracts of the same instFamily can't be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, current sell order size: {param4} contracts, current instId position quantity: {param5} contracts, current instId pending sell orders: {param6} contracts, other contracts of the same instFamily: {param7} contracts). |
| 51004 | 200 | Order amendment failed. For isolated long/short mode of {param0}, the sum of increment order size by amendment, position quantity in the same direction, and pending orders in the same direction can't be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, increment order size by amendment: {param3} contracts, position quantity in the same direction: {param4} contracts, pending orders in the same direction: {param5} contracts). |
| 51004 | 200 | Order amendment failed. For cross long/short mode of {param0}, the sum of increment order size by amendment, position quantity in the long and short directions, and pending orders in the long and short directions can't be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, increment order size by amendment: {param3} contracts, position quantity in the long and short directions: {param4} contracts, pending orders in the same direction: {param5} contracts). |
| 51004 | 200 | Order amendment failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of increment order size by amendment, current instId position quantity in the long and short directions, current instId pending orders in the long and short directions, and other contracts of the same instFamily can't be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, increment order size by amendment: {param4} contracts, current instId position quantity in the long and short directions: {param5} contracts, current instId pending orders in the long and short directions: {param6} contracts, other contracts of the same instFamily: {param7} contracts). |
| 51004 | 200 | Order amendment failed. For buy/sell mode of {param0}, the sum of increment order size by amending current buy order, position quantity, and pending buy orders can't be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, increment order size by amending current buy order: {param3} contracts, position quantity: {param4} contracts, pending buy orders: {param5} contracts). |
| 51004 | 200 | Order amendment failed. For buy/sell mode of {param0}, the sum of increment order size by amending current sell order, position quantity, and pending sell orders can't be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, increment order size by amending current sell order: {param3} contracts, position quantity: {param4} contracts, pending sell orders: {param5} contracts). |
| 51004 | 200 | Order amendment failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of increment order size by amending current buy order, current instId position quantity, current instId pending buy orders, and other contracts of the same instFamily can't be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, increment order size by amending current buy order: {param4} contracts, current instId position quantity: {param5} contracts, current instId pending buy orders: {param6} contracts, other contracts of the same instFamily: {param7} contracts). |
| 51004 | 200 | Order amendment failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of increment order size by amending current sell order, current instId position quantity, current instId pending sell orders, and other contracts of the same instFamily can't be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, increment order size by amending current sell order: {param4} contracts, current instId position quantity: {param5} contracts, current instId pending sell orders: {param6} contracts, other contracts of the same instFamily: {param7} contracts). |
| 51005 | 200 | Your order amount exceeds the max order amount. |
| 51006 | 200 | Order price is not within the price limit (max buy price: {param0} , min sell price: {param1} ) |
| 51007 | 200 | Order failed. Please place orders of at least 1 contract or more. |
| 51008 | 200 | Order failed. Insufficient {param0} balance in account |
| 51008 | 200 | Order failed. Insufficient {param0} margin in account |
| 51008 | 200 | Order failed. Insufficient {param0} balance in account, and Auto Borrow is not enabled |
| 51008 | 200 | Order failed. Insufficient {param0} margin in account and auto-borrow is not enabled (Portfolio margin mode can try IOC orders to lower the risks) |
| 51008 | 200 | Insufficient {param0} available as your borrowing amount exceeds tier limit. Lower leverage appropriately. New and pending limit orders need borrowings {param1}, remaining quota {param2}, total limit {param3}, in use {param4}. |
| 51008 | 200 | Order failed. Exceeds {param0} borrow limit (Limit of master account plus the allocated VIP quota for the current account) (Existing pending orders and the new order are required to borrow {param1}, Remaining limit {param2}, Limit {param3}, Limit used {param4}) |
| 51008 | 200 | Order failed. Insufficient {param0} borrowing quota results in an insufficient amount available to borrow. |
| 51008 | 200 | Order failed. Insufficient {param0} available in loan pool to borrow. |
| 51008 | 200 | Order failed. Insufficient account balance, and the adjusted equity in USD is less than IMR (Portfolio margin mode can try IOC orders to lower the risks) |
| 51008 | 200 | Order failed. The order didn't pass delta verification because if the order were to succeed, the change in adjEq would be smaller than the change in IMR. Increase adjEq or reduce IMR (Portfolio margin mode can try IOC orders to lower the risks) |
| 51009 | 200 | Order blocked. Please contact customer support for assistance. |
| 51010 | 200 | Request unsupported under current account mode |
| 51011 | 200 | Order ID already exists. |
| 51012 | 200 | Token doesn't exist. |
| 51014 | 200 | Index doesn't exist. |
| 51015 | 200 | Instrument ID doesn't match instrument type. |
| 51016 | 200 | Client order ID already exists. |
| 51017 | 200 | Loan amount exceeds borrowing limit. |
| 51018 | 200 | Users with options accounts cannot hold net short positions. |
| 51019 | 200 | No net long positions can be held under cross margin mode in options. |
| 51020 | 200 | Your order should meet or exceed the minimum order amount. |
| 51021 | 200 | The pair or contract is not yet listed |
| 51022 | 200 | Contract suspended. |
| 51023 | 200 | Position doesn't exist. |
| 51024 | 200 | Trading account is blocked. |
| 51024 | 200 | In accordance with the terms of service, we regret to inform you that we cannot provide services for you. If you have any questions, please contact our customer support. |
| 51024 | 200 | According to your request, this account has been frozen. If you have any questions, please contact our customer support. |
| 51024 | 200 | Your account has recently changed some security settings. To protect the security of your funds, this action is not allowed for now. If you have any questions, please contact our customer support. |
| 51024 | 200 | You have withdrawn all assets in the account. To protect your personal information, the account has been permanently frozen. If you have any questions, please contact our customer support. |
| 51024 | 200 | Your identity could not be verified. To protect the security of your funds, this action is not allowed. Please contact our customer support. |
| 51024 | 200 | Your verified age doesn't meet the requirement. To protect the security of your funds, we cannot proceed with your request. Please contact our customer support. |
| 51024 | 200 | In accordance with the terms of service, trading is currently unavailable in your verified country or region. Close all open positions or contact customer support if you have any questions. |
| 51024 | 200 | In accordance with the terms of service, multiple account is not allowed. To protect the security of your funds, this action is not allowed. Please contact our customer support. |
| 51024 | 200 | Your account is in judicial freezing, and this action is not allowed for now. If you have any questions, please contact our customer support. |
| 51024 | 200 | Based on your previous requests, this action is not allowed for now. If you have any questions, please contact our customer support. |
| 51024 | 200 | Your account has disputed deposit orders. To protect the security of your funds, this action is not allowed for now. Please contact our customer support. |
| 51024 | 200 | Unable to proceed. Please resolve your existing P2P disputes first. |
| 51024 | 200 | Your account might have compliance risk. To protect the security of your funds, this action is not allowed for now. Please contact our customer support. |
| 51024 | 200 | Based on your trading requests, this action is not allowed for now. If you have any questions, please contact our customer support. |
| 51024 | 200 | Your account has triggered risk control. This action is not allowed for now. Please contact our customer support. |
| 51024 | 200 | This account is temporarily unavailable. Please contact our customer support. |
| 51024 | 200 | Withdrawal function of this account is temporarily unavailable. Please contact our customer support. |
| 51024 | 200 | Transfer function of this account is temporarily unavailable. Please contact our customer support. |
| 51024 | 200 | You violated the "Fiat Trading Rules" when you were doing fiat trade, so we'll no longer provide fiat trading-related services for you. The deposit and withdrawal of your account and other trading functions will not be affected. |
| 51024 | 200 | Please kindly check your mailbox and reply to emails from the verification team. |
| 51024 | 200 | According to your request, this account has been closed. If you have any questions, please contact our customer support. |
| 51024 | 200 | Your account might have security risk. To protect the security of your funds, this action is not allowed for now. Please contact our customer support. |
| 51024 | 200 | Your account might have security risk. Convert is now unavailable. Please contact our customer support. |
| 51024 | 200 | Unable to proceed due to account restrictions. We've sent an email to your OKX registered email address regarding this matter, or you can contact customer support via Chat with AI chatbot on our support center page. |
| 51024 | 200 | In accordance with the terms of service, trading is currently unavailable in your verified country or region. Cancel all orders or contact customer support if you have any questions. |
| 51024 | 200 | In accordance with the terms of service, trading is not available in your verified country. If you have any questions, please contact our customer support. |
| 51024 | 200 | This product isn't available in your country or region due to local laws and regulations. If you don't reside in this area, you may continue using OKX Exchange products with a valid government-issued ID. |
| 51024 | 200 | Please note that you may not be able to transfer or trade in the first 30 minutes after establishing custody trading sub-accounts. Please kindly wait and try again later. |
| 51024 | 200 | Feature unavailable. Complete Advanced verification to access this feature. |
| 51024 | 200 | You can't trade or deposit now. Update your personal info to restore full account access immediately. |
| 51024 | 200 | Sub-accounts exceeding the limit aren't allowed to open new positions and can only reduce or close existing ones. Please try again with a different account. |
| 51025 | 200 | Order count exceeds the limit. |
| 51026 | 200 | Instrument type doesn't match underlying index. |
| 51027 | 200 | Contract expired. |
| 51028 | 200 | Contract under delivery. |
| 51029 | 200 | Contract is being settled. |
| 51030 | 200 | Funding fee is being settled. |
| 51031 | 200 | This order price is not within the closing price range. |
| 51032 | 200 | Closing all the positions at the market price. |
| 51033 | 200 | The total amount per order for this pair has reached the upper limit. |
| 51034 | 200 | Fill rate exceeds the limit that you've set. Please reset the market maker protection to inactive for new trades. |
| 51035 | 200 | This account doesn't have permission to submit MM quote order. |
| 51036 | 200 | Only options instrument of the PM account supports MMP orders. |
| 51037 | 200 | This account only supports placing IOC orders to reduce account risk. |
| 51038 | 200 | IOC order already exists under the current risk module. |
| 51039 | 200 | Leverage cannot be adjusted for the cross positions of Expiry Futures and Perpetual Futures under the PM account. |
| 51040 | 200 | Cannot adjust margins for long isolated options positions |
| 51041 | 200 | Portfolio margin account only supports the Buy/Sell mode. |
| 51042 | 200 | Under the Portfolio margin account, users can only place market maker protection orders in cross margin mode in Options. |
| 51043 | 200 | This isolated position doesn't exist. |
| 51044 | 200 | The order type {param0}, {param1} is not allowed to set stop loss and take profit |
| 51046 | 200 | The take profit trigger price must be higher than the order price |
| 51047 | 200 | The stop loss trigger price must be lower than the order price |
| 51048 | 200 | The take profit trigger price must be lower than the order price |
| 51049 | 200 | The stop loss trigger price must be higher than the order price |
| 51050 | 200 | The take profit trigger price must be higher than the best ask price |
| 51051 | 200 | The stop loss trigger price must be lower than the best ask price |
| 51052 | 200 | The take profit trigger price must be lower than the best bid price |
| 51053 | 200 | The stop loss trigger price must be higher than the best bid price |
| 51054 | 500 | Request timed out. Please try again. |
| 51055 | 200 | Futures Grid is not available in Portfolio Margin mode |
| 51056 | 200 | Action not allowed |
| 51057 | 200 | This bot isn't available in current account mode. Switch mode in Settings > Account mode to continue. |
| 51058 | 200 | No available position for this algo order |
| 51059 | 200 | Strategy for the current state does not support this operation |
| 51063 | 200 | OrdId does not exist |
| 51065 | 200 | algoClOrdId already exists. |
| 51066 | 200 | Market orders unavailable for options trading. Place a limit order to close position. |
| 51068 | 200 | {param0} already exists within algoClOrdId and attachAlgoClOrdId. |
| 51069 | 200 | The option contracts related to current {param0} do not exist |
| 51070 | 200 | You do not meet the requirements for switching to this account mode. Please upgrade the account mode on the OKX website or App |
| 51071 | 200 | You've reached the maximum limit for tag level cancel all after timers. |
| 51072 | 200 | As a spot lead trader, you need to set tdMode to spot_isolated when buying the configured lead trade pairs. |
| 51073 | 200 | As a spot lead trader, you need to use '/copytrading/close-subposition' for selling assets through lead trades |
| 51074 | 200 | Only the tdMode for lead trade pairs configured by spot lead traders can be set to 'spot_isolated' |
| 51075 | 200 | Order modification failed. You can only modify the price of sell orders in spot copy trading. |
| 51076 | 200 | TP/SL orders in Split TPs only support one-way TP/SL. You can't use slTriggerPx&slOrdPx and tpTriggerPx&tpOrdPx at the same time. |
| 51077 | 200 | Setting multiple TP and cost-price SL orders isn't supported for spot and margin trading. |
| 51078 | 200 | You are a lead trader. Split TPs are not supported. |
| 51079 | 200 | The number of TP orders with Split TPs attached in a same order cannot exceed {param0} |
| 51080 | 200 | Take-profit trigger price types (tpTriggerPxType) must be the same in an order with Split TPs attached |
| 51081 | 200 | Take-profit trigger prices (tpTriggerPx) cannot be the same in an order with Split TPs attached |
| 51082 | 200 | TP trigger prices (tpOrdPx) in one order with multiple TPs must be market prices. |
| 51083 | 200 | The total size of TP orders with Split TPs attached in a same order should equal the size of this order |
| 51084 | 200 | The number of SL orders with Split TPs attached in a same order cannot exceed {param0} |
| 51085 | 200 | The number of TP orders cannot be less than 2 when cost-price SL is enabled (amendPxOnTriggerType set as 1) for Split TPs |
| 51086 | 200 | The number of orders with Split TPs attached in a same order cannot exceed {param0} |
| 51087 | 200 | Listing canceled for this crypto |
| 51088 | 200 | You can only place 1 TP/SL order to close an entire position |
| 51089 | 200 | The size of the TP order among split TPs attached cannot be empty |
| 51090 | 200 | You can't modify the amount of an SL order placed with a TP limit order. |
| 51091 | 200 | All TP orders in one order must be of the same type. |
| 51092 | 200 | TP order prices (tpOrdPx) in one order must be different. |
| 51093 | 200 | TP limit order prices (tpOrdPx) in one order can't be –1 (market price). |
| 51094 | 200 | You can't place TP limit orders in spot, margin, or options trading. |
| 51095 | 200 | To place TP limit orders at this endpoint, you must place an SL order at the same time. |
| 51096 | 200 | cxlOnClosePos needs to be true to place a TP limit order |
| 51098 | 200 | You can't add a new TP order to an SL order placed with a TP limit order. |
| 51099 | 200 | You can't place TP limit orders as a lead trader. |
| 51100 | 200 | Unable to place order. Take profit/Stop loss conditions cannot be added to reduce-only orders. |
| 51101 | 200 | Order failed, the sz of the current order can't be more than {param0} (contracts). |
| 51102 | 200 | Order failed, the number of pending orders for this instId can't be more than {param0} (orders) |
| 51103 | 200 | Order failed, the number of pending orders across all instIds under the {param0} current instFamily can't be more than {param1} (orders) |
| 51104 | 200 | Order failed, the aggregated contract quantity for all pending orders across all instIds under the {param0} current instFamily can't be more than {param1} (contracts) |
| 51105 | 200 | Order failed, the maximal sum of position quantity and pending orders quantity with the same direction for current instId can't be more than {param0} (contracts) |
| 51106 | 200 | Order failed, the maximal sum of position quantity and pending orders quantity with the same direction across all instIds under the {param0} current instFamily can't be more than {param1} (contracts) |
| 51107 | 200 | Order failed, the maximal sum of position quantity and pending orders quantity in both directions across all instIds under the {param0} current instFamily can't be more than {param1} (contracts) |
| 51108 | 200 | Positions exceed the limit for closing out with the market price. |
| 51109 | 200 | No available offers. |
| 51110 | 200 | You can only place a limit order after Call Auction has started. |
| 51111 | 200 | Maximum {param0} orders can be placed in bulk. |
| 51112 | 200 | Close order size exceeds available size for this position. |
| 51113 | 429 | Market-price liquidation requests too frequent. |
| 51115 | 429 | Cancel all pending close-orders before liquidation. |
| 51116 | 200 | Order price or trigger price exceeds {param0}. |
| 51117 | 200 | Pending close-orders count exceeds limit. |
| 51120 | 200 | Order amount is less than {param0}, please try again. |
| 51121 | 200 | Order quantity must be a multiple of the lot size. |
| 51122 | 200 | Order price should higher than the min price {param0} |
| 51123 | 200 | Min price increment is null. |
| 51124 | 200 | You can only place limit orders during call auction. |
| 51125 | 200 | Currently there are pending reduce + reverse position orders in margin trading. Please cancel all pending reduce + reverse position orders and continue. |
| 51126 | 200 | Currently there are pending reduce only orders in margin trading. Please cancel all pending reduce only orders and continue. |
| 51127 | 200 | Available balance is 0. |
| 51128 | 200 | Multi-currency margin accounts cannot do cross-margin trading. |
| 51129 | 200 | The value of the position and buy order has reached the position limit. No further buying is allowed. |
| 51130 | 200 | Fixed margin currency error. |
| 51131 | 200 | Insufficient balance. |
| 51132 | 200 | Your position amount is negative and less than the minimum trading amount. |
| 51133 | 200 | Reduce-only feature is unavailable for spot transactions in multi-currency margin accounts. |
| 51134 | 200 | Closing failed. Please check your margin holdings and pending orders. Turn off the Reduce-only to continue. |
| 51135 | 200 | Your closing price has triggered the limit price, and the max buy price is {param0}. |
| 51136 | 200 | Your closing price has triggered the limit price, and the min sell price is {param0}. |
| 51137 | 200 | The highest price limit for buy orders is {param0}. |
| 51138 | 200 | The lowest price limit for sell orders is {param0}. |
| 51139 | 200 | Reduce-only feature is unavailable for the spot transactions by spot mode. |
| 51140 | 200 | Purchase failed due to insufficient sell orders, please try later. |
| 51142 | 200 | There is no valid quotation in the market, and the order cannot be filled in USDT mode, please try to switch to currency mode |
| 51143 | 200 | Insufficient conversion amount |
| 51144 | 200 | Please use {param0} for closing. |
| 51147 | 200 | To trade options, make sure you have more than 10,000 USD worth of assets in your trading account first, then activate options trading |
| 51148 | 200 | Failed to place order. The new order may execute an opposite trading direction of your existing reduce-only positions. Cancel or edit pending orders to continue order |
| 51149 | 500 | Order timed out. Please try again. |
| 51150 | 200 | The precision of the number of trades or the price exceeds the limit. |
| 51152 | 200 | Unable to place an order that mixes automatic buy with automatic repayment or manual operation in Quick margin mode. |
| 51153 | 200 | Unable to borrow manually in Quick margin mode. The amount you entered exceeds the upper limit. |
| 51154 | 200 | Unable to repay manually in Quick margin mode. The amount you entered exceeds your available balance. |
| 51155 | 200 | Trading of this pair or contract is restricted due to local compliance requirements. |
| 51156 | 200 | You're leading trades in long/short mode and can't use this API endpoint to close positions |
| 51158 | 200 | Manual transfer unavailable. To proceed, please switch to Quick margin mode (isoMode = quick_margin) |
| 51159 | 200 | You're leading trades in buy/sell mode. If you want to place orders using this API endpoint, the orders must be in the same direction as your existing positions and open orders. |
| 51162 | 200 | You have {instrument} open orders. Cancel these orders and try again |
| 51163 | 200 | You hold {instrument} positions. Close these positions and try again |
| 51164 | 200 | As lead trader, you can't switch to portfolio margin mode. |
| 51165 | 200 | The number of {instrument} reduce-only orders reached the upper limit of {upLimit}. Cancel some orders to proceed. |
| 51166 | 200 | Currently, we don't support leading trades with this instrument |
| 51167 | 200 | Failed. You have block trading open order(s), please proceed after canceling existing order(s). |
| 51168 | 200 | Failed. You have reduce-only type of open order(s), please proceed after canceling existing order(s) |
| 51169 | 200 | Order failed because you don't have any positions in this direction for this contract to reduce or close. |
| 51170 | 200 | Failed to place order. A reduce-only order can't be the same trading direction as your existing positions. |
| 51171 | 200 | Failed to edit order. The edited order may execute an opposite trading direction of your existing reduce-only positions. Cancel or edit pending orders to continue. |
| 51173 | 200 | Unable to close all at market price. Your current positions don't have any liabilities. |
| 51174 | 200 | Order failed, number of pending orders for {param0} exceed the limit of {param1}. |
| 51175 | 200 | Parameters {param0} {param1} and {param2} cannot be empty at the same time |
| 51176 | 200 | Only one parameter can be filled among Parameters {param0} {param1} and {param2} |
| 51177 | 200 | Unavailable to amend {param1} because the price type of the current options order is {param0} |
| 51178 | 200 | tpTriggerPx&tpOrdPx or slTriggerPx&slOrdPx can't be empty when using attachAlgoClOrdId. |
| 51179 | 200 | Unavailable to place options orders using {param0} in simple mode |
| 51180 | 200 | The range of {param0} should be ({param1}~{param2}) |
| 51181 | 200 | ordType must be limit while placing {param0} orders |
| 51182 | 200 | The total number of pending orders under price types pxUsd and pxVol for the current account cannot exceed {param0} |
| 51185 | 200 | The maximum value allowed per order is {maxOrderValue} USD |
| 51186 | 200 | Order failed. The leverage for {param0} in your current margin mode is {param1}x, which exceeds the platform limit of {param2}x. |
| 51187 | 200 | Order failed. For {param0} {param1} in your current margin mode, the sum of your current order amount, position sizes, and open orders is {param2} contracts, which exceeds the platform limit of {param3} contracts. Reduce your order amount, cancel orders, or close positions. |
| 51192 | 200 | The {param0} price corresponding to the IV level you entered is lower than the minimum allowed selling price of {param1} {param2}. Enter a higher IV level. |
| 51193 | 200 | The {param0} price corresponding to the IV level you entered is higher than the maximum allowed buying price of {param1} {param2}. Enter a lower IV level. |
| 51194 | 200 | The {param0} price corresponding to the USD price you entered is lower than the minimum allowed selling price of {param1} {param2}. Enter a higher USD price. |
| 51195 | 200 | The {param0} price corresponding to the USD price you entered is higher than the maximum allowed buying price of {param1} {param2}. Enter a lower USD price. |
| 51196 | 200 | You can only place limit orders during the pre-quote phase. |
| 51197 | 200 | You can only place limit orders after the pre-quote phase begins. |
| 51201 | 200 | The value of a market order can't exceed {param0}. |
| 51202 | 200 | Market order amount exceeds the maximum amount. |
| 51203 | 200 | Order amount exceeds the limit {param0}. |
| 51204 | 200 | The price for the limit order cannot be empty. |
| 51205 | 200 | Reduce Only is not available. |
| 51206 | 200 | Please cancel the Reduce Only order before placing the current {param0} order to avoid opening a reverse position. |
| 51207 | 200 | Trading amount exceeds the limit, and can't be all closed at the market price. You can try closing the position manually in batches. |
| 51220 | 200 | Lead and follow bots only support "Sell" or "Close all positions" when bot stops |
| 51221 | 200 | The profit-sharing ratio must be between 0% and 30% |
| 51222 | 200 | Profit sharing isn't supported for this type of bot |
| 51223 | 200 | Only lead bot creators can set profit-sharing ratio |
| 51224 | 200 | Profit sharing isn't supported for this crypto pair |
| 51225 | 200 | Instant trigger isn't available for follow bots |
| 51226 | 200 | Editing parameters isn't available for follow bots |
| 51250 | 200 | Algo order price is out of the available range. |
| 51251 | 200 | Bot order type error occurred when placing iceberg order |
| 51252 | 200 | Algo order amount is out of the available range. |
| 51253 | 200 | Average amount exceeds the limit of per iceberg order. |
| 51254 | 200 | Iceberg average amount error occurred. |
| 51255 | 200 | Limit of per iceberg order: Total amount/1000 < x <= Total amount. |
| 51256 | 200 | Iceberg order price variance error. |
| 51257 | 200 | Trailing stop order callback rate error. The callback rate should be {min}< x<={max}%. |
| 51258 | 200 | Trailing stop order placement failed. The trigger price of a sell order must be higher than the last transaction price. |
| 51259 | 200 | Trailing stop order placement failed. The trigger price of a buy order must be lower than the last transaction price. |
| 51260 | 200 | Maximum of {param0} pending trailing stop orders can be held at the same time. |
| 51261 | 200 | Each user can hold up to {param0} pending stop orders at the same time. |
| 51262 | 200 | Maximum {param0} pending iceberg orders can be held at the same time. |
| 51263 | 200 | Maximum {param0} pending time-weighted orders can be held at the same time. |
| 51264 | 200 | Average amount exceeds the limit of per time-weighted order. |
| 51265 | 200 | Time-weighted order limit error. |
| 51267 | 200 | Time-weighted order strategy initiative rate error. |
| 51268 | 200 | Time-weighted order strategy initiative range error. |
| 51269 | 200 | Time-weighted order interval error. Interval must be {%min}<= x<={%max}. |
| 51270 | 200 | The limit of time-weighted order price variance is 0 < x <= 1%. |
| 51271 | 200 | Sweep ratio must be 0 < x <= 100%. |
| 51272 | 200 | Price variance must be 0 < x <= 1%. |
| 51273 | 200 | Total amount must be greater than {param0}. |
| 51274 | 200 | Total quantity of time-weighted order must be larger than single order limit. |
| 51275 | 200 | The amount of single stop-market order cannot exceed the upper limit. |
| 51276 | 200 | Prices cannot be specified for stop market orders. |
| 51277 | 200 | TP trigger price cannot be higher than the last price. |
| 51278 | 200 | SL trigger price cannot be lower than the last price. |
| 51279 | 200 | TP trigger price cannot be lower than the last price. |
| 51280 | 200 | SL trigger price cannot be higher than the last price. |
| 51281 | 200 | Trigger order do not support the tgtCcy parameter. |
| 51282 | 200 | The range of Price variance is {param0}~{param1} |
| 51283 | 200 | The range of Time interval is {param0}~{param1} |
| 51284 | 200 | The range of Average amount is {param0}~{param1} |
| 51285 | 200 | The range of Total amount is {param0}~{param1} |
| 51286 | 200 | The total amount should not be less than {param0} |
| 51287 | 200 | This bot doesn't support current instrument |
| 51288 | 200 | Bot is currently stopping. Do not make multiple attempts to stop. |
| 51289 | 200 | Bot configuration does not exist. Please try again later |
| 51290 | 200 | The Bot engine is being upgraded. Please try again later |
| 51291 | 200 | This Bot does not exist or has been stopped |
| 51292 | 200 | This Bot type does not exist |
| 51293 | 200 | This Bot does not exist |
| 51294 | 200 | This Bot cannot be created temporarily. Please try again later |
| 51295 | 200 | Portfolio margin account does not support ordType {param0} in Trading bot mode |
| 51298 | 200 | Trigger orders are not available in the net mode of Expiry Futures and Perpetual Futures |
| 51299 | 200 | Order did not go through. You can hold a maximum of {param0} orders of this type. |
| 51300 | 200 | TP trigger price cannot be higher than the mark price |
| 51302 | 200 | SL trigger price cannot be lower than the mark price |
| 51303 | 200 | TP trigger price cannot be lower than the mark price |
| 51304 | 200 | SL trigger price cannot be higher than the mark price |
| 51305 | 200 | TP trigger price cannot be higher than the index price |
| 51306 | 200 | SL trigger price cannot be lower than the index price |
| 51307 | 200 | TP trigger price cannot be lower than the index price |
| 51308 | 200 | SL trigger price cannot be higher than the index price |
| 51309 | 200 | Cannot create trading bot during call auction |
| 51310 | 200 | Strategic orders with Iceberg and TWAP order type are not supported when margins are self-transferred in isolated mode. |
| 51311 | 200 | Failed to place trailing stop order. Callback rate should be within {min}<x<={max} |
| 51312 | 200 | Failed to place trailing stop order. Order amount should be within {min}<x<={max} |
| 51313 | 200 | Manual transfer in isolated mode does not support bot trading |
| 51317 | 200 | Trigger orders are not available by margin |
| 51320 | 200 | The range of coin percentage is {0}%-{1}% |
| 51321 | 200 | You're leading trades. Currently, we don't support leading trades with arbitrage, iceberg, or TWAP bots |
| 51322 | 200 | You're leading trades that have been filled at market price. We've canceled your open stop orders to close your positions |
| 51323 | 200 | You're already leading trades with take profit or stop loss settings. Cancel your existing stop orders to proceed |
| 51324 | 200 | As a lead trader, you hold positions in {instrument}. To close your positions, place orders in the amount that equals the available amount for closing |
| 51325 | 200 | As a lead trader, you must use market price when placing stop orders |
| 51326 | 200 | As a lead trader, you must use market price when placing orders with take profit or stop loss settings |
| 51327 | 200 | closeFraction is only available for Expiry Futures and Perpetual Futures |
| 51328 | 200 | closeFraction is only available for reduceOnly orders |
| 51329 | 200 | closeFraction is only available in NET mode |
| 51330 | 200 | closeFraction is only available for stop market orders |
| 51331 | 200 | closeFraction is only available for close position orders |
| 51332 | 200 | closeFraction is not applicable to Portfolio Margin |
| 51333 | 200 | Close position order in hedge-mode or reduce-only order in one-way mode cannot attach TPSL |
| 51340 | 200 | Used margin must be greater than {0}{1} |
| 51341 | 200 | Position closing not allowed |
| 51342 | 200 | Closing order already exists. Please try again later |
| 51343 | 200 | TP price must be less than the lower price |
| 51344 | 200 | SL price must be greater than the upper price |
| 51345 | 200 | Policy type is not grid policy |
| 51346 | 200 | The highest price cannot be lower than the lowest price |
| 51347 | 200 | No profit available |
| 51348 | 200 | Stop loss price must be less than the lower price in the range. |
| 51349 | 200 | Take profit price must be greater than the highest price in the range. |
| 51350 | 200 | No recommended parameters |
| 51351 | 200 | Single income must be greater than 0 |
| 51352 | 200 | You can have {0} to {1} trading pairs |
| 51353 | 200 | Trading pair {0} already exists |
| 51354 | 200 | The percentages of all trading pairs should add up to 100% |
| 51355 | 200 | Select a date within {0} - {1} |
| 51356 | 200 | Select a time within {0} - {1} |
| 51357 | 200 | Select a time zone within {0} - {1} |
| 51358 | 200 | The investment amount of each crypto must be greater than {amount} |
| 51359 | 200 | Recurring buy not supported for the selected crypto {0} |
| 51370 | 200 | The range of lever is {0}~{1} |
| 51380 | 200 | Market conditions do not meet the strategy running configuration. You can try again later or adjust your tp/sl configuration. |
| 51381 | 200 | Per grid profit ratio must be larger than 0.1% and less or equal to 10% |
| 51382 | 200 | Stop triggerAction is not supported by the current strategy |
| 51383 | 200 | The min_price is lower than the last price |
| 51384 | 200 | The trigger price must be greater than the min price |
| 51385 | 200 | The take profit price needs to be greater than the min price |
| 51386 | 200 | The min price needs to be greater than 1/2 of the last price |
| 51387 | 200 | Stop loss price must be less than the bottom price |
| 51388 | 200 | This Bot is in running status |
| 51389 | 200 | Trigger price should be lower than {0} |
| 51390 | 200 | Trigger price should be lower than the TP price |
| 51391 | 200 | Trigger price should be higher than the SL price |
| 51392 | 200 | TP price should be higher than the trigger price |
| 51393 | 200 | SL price should be lower than the trigger price |
| 51394 | 200 | Trigger price should be higher than the TP price |
| 51395 | 200 | Trigger price should be lower than the SL price |
| 51396 | 200 | TP price should be lower than the trigger price |
| 51397 | 200 | SL price should be higher than the trigger price |
| 51398 | 200 | Current market meets the stop condition. The bot cannot be created. |
| 51399 | 200 | Max margin under current leverage: {amountLimit} {quoteCurrency}. Enter a smaller amount and try again. |
| 51400 | 200 | Order cancellation failed as the order has been filled, canceled or does not exist. |
| 51400 | 200 | Cancellation failed as the order does not exist. (Only applicable to Nitro Spread) |
| 51401 | 200 | Cancellation failed as the order is already canceled. (Only applicable to Nitro Spread) |
| 51402 | 200 | Cancellation failed as the order is already completed. (Only applicable to Nitro Spread) |
| 51403 | 200 | Cancellation failed as the order type doesn't support cancellation. |
| 51404 | 200 | Order cancellation unavailable during the second phase of call auction. |
| 51405 | 200 | Cancellation failed as you don't have any pending orders. |
| 51406 | 400 | Canceled - order count exceeds the limit {param0}. |
| 51407 | 200 | Either order ID or client order ID is required. |
| 51408 | 200 | Pair ID or name doesn't match the order info. |
| 51409 | 200 | Either pair ID or pair name ID is required. |
| 51410 | 200 | Cancellation failed as the order is already in canceling status or pending settlement. |
| 51411 | 200 | Account does not have permission for mass cancellation. |
| 51412 | 200 | Cancellation timed out, please try again later. |
| 51412 | 200 | The order has been triggered and can't be canceled. |
| 51413 | 200 | Cancellation failed as the order type is not supported by endpoint. |
| 51415 | 200 | Unable to place order. Spot trading only supports using the last price as trigger price. Please select "Last" and try again. |
| 51416 | 200 | Order has been triggered and can't be canceled |
| 51500 | 200 | You must enter a price, quantity, or TP/SL |
| 51501 | 400 | Maximum {param0} orders can be modified. |
| 51502 | 200 | Unable to edit order: insufficient balance or margin. |
| 51502 | 200 | Order failed. Insufficient {param0} margin in account |
| 51502 | 200 | Order failed. Insufficient {param0} balance in account and Auto Borrow is not enabled |
| 51502 | 200 | Order failed. Insufficient {param0} margin in account and Auto Borrow is not enabled (Portfolio margin mode can try IOC orders to lower the risks) |
| 51502 | 200 | Order failed. The requested borrowing amount is larger than the available {param0} borrowing amount of your position tier. Existing pending orders and the new order need to borrow {param1}, remaining quota {param2}, total quota {param3}, used {param4} |
| 51502 | 200 | Order failed. The requested borrowing amount is larger than the available {param0} borrowing amount of your position tier. Existing pending orders and the new order need to borrow {param1}, remaining quota {param2}, total quota {param3}, used {param4} |
| 51502 | 200 | Order failed. The requested borrowing amount is larger than the available {param0} borrowing amount of your main account and the allocated VIP quota. Existing pending orders and the new order need to borrow {param1}, remaining quota {param2}, total quota {param3}, used {param4} |
| 51502 | 200 | Order failed. Insufficient available borrowing amount in {param0} crypto pair |
| 51502 | 200 | Order failed. Insufficient available borrowing amount in {param0} loan pool |
| 51502 | 200 | Order failed. Insufficient account balance and the adjusted equity in USD is smaller than the IMR. |
| 51502 | 200 | Order failed. The order didn't pass delta verification. If the order succeeded, the change in adjEq would be smaller than the change in IMR. Increase adjEq or reduce IMR (Portfolio margin mode can try IOC orders to lower the risks) |
| 51503 | 200 | Your order has already been filled or canceled. |
| 51503 | 200 | Order modification failed as the order does not exist. (Only applicable to Nitro Spread) |
| 51505 | 200 | {instId} is not in call auction |
| 51506 | 200 | Order modification unavailable for the order type. |
| 51507 | 200 | You can only place market orders for this crypto at least 5 minutes after its listing. |
| 51508 | 200 | Orders are not allowed to be modified during the call auction. |
| 51509 | 200 | Modification failed as the order has been canceled. (Only applicable to Nitro Spread) |
| 51510 | 200 | Modification failed as the order has been completed. (Only applicable to Nitro Spread) |
| 51511 | 200 | Modification failed as the order price did not meet the requirement for Post Only. |
| 51512 | 200 | Failed to amend orders in batches. You cannot have duplicate orders in the same amend-batch-orders request. |
| 51513 | 200 | Number of modification requests that are currently in progress for an order cannot exceed 3 times. |
| 51514 | 200 | Order modification failed. The price length must be 32 characters or shorter. |
| 51521 | 200 | Failed to edit. Unable to edit reduce-only order because you don't have any positions of this contract. |
| 51522 | 200 | Failed to edit. A reduce-only order can't be in the same trading direction as your existing positions. |
| 51523 | 200 | Unable to modify the order price of a stop order that closes an entire position. Please modify the trigger price instead. |
| 51524 | 200 | Unable to modify the order quantity of a stop order that closes an entire position. Please modify the trigger price instead. |
| 51525 | 200 | Stop order modification is not available for quick margin |
| 51526 | 200 | Order modification unsuccessful. Take profit/Stop loss conditions cannot be added to or removed from stop orders. |
| 51527 | 200 | Order modification unsuccessful. The stop order does not exist. |
| 51527 | 200 | Order modification failed. At least 1 of the attached TP/SL orders does not exist. |
| 51528 | 200 | Unable to modify trigger price type |
| 51529 | 200 | Order modification unsuccessful. Stop order modification only applies to Expiry Futures and Perpetual Futures. |
| 51530 | 200 | Order modification unsuccessful. Take profit/Stop loss conditions cannot be added to or removed from reduce-only orders. |
| 51531 | 200 | Order modification unsuccessful. The stop order must have either take profit or stop loss attached. |
| 51532 | 200 | Your TP/SL can't be modified because it was partially triggered |
| 51536 | 200 | Unable to modify the size of the options order if the price type is pxUsd or pxVol. |
| 51537 | 200 | pxUsd or pxVol are not supported by non-options instruments |
| 51543 | 200 | When modifying take-profit or stop-loss orders for spot or margin trading, you can only adjust the price and quantity. Cancel the order and place a new one for other actions.                                          |
| 51600 | 200 | Status not found.                                                                                                                                                                                                                                                                                    |
| 51601 | 200 | Order status and order id cannot exist at the same time.                                                                                                                                                                                                                                             |
| 51602 | 200 | Either order status or order ID is required.                                                                                                                                                                                                                                                         |
| 51603 | 200 | Order does not exist.                                                                                                                                                                                                                                                                                |
| 51604 | 200 | Initiate a download request before obtaining the hyperlink                                                                                                                                                                                                                                           |
| 51605 | 200 | You can only download transaction data from the past 2 years                                                                                                                                                                                                                                         |
| 51606 | 200 | Transaction data for the current quarter is not available                                                                                                                                                                                                                                            |
| 51607 | 200 | Your previous download request is still being processed                                                                                                                                                                                                                                              |
| 51608 | 200 | No transaction data found for the current quarter                                                                                                                                                                                                                                                    |
| 51610 | 200 | You can't download billing statements for the current quarter.                                                                                                                                                                                                                                       |
| 51611 | 200 | You can't download billing statements for the current quarter.                                                                                                                                                                                                                                       |
| 51620 | 200 | Only affiliates can perform this action                                                                                                                                                                                                                                                              |
| 51621 | 200 | The user isn’t your invitee                                                                                                                                                                                                                                                                          |
| 51156 | 200 | You're leading trades in long/short mode and can't use this API endpoint to close positions                                                                                                                                                                                                          |
| 51159 | 200 | You're leading trades in buy/sell mode. If you want to place orders using this API endpoint, the orders must be in the same direction as your existing positions and open orders.                                                                                                                    |
| 51162 | 200 | You have {instrument} open orders. Cancel these orders and try again                                                                                                                                                                                                                                 |
| 51163 | 200 | You hold {instrument} positions. Close these positions and try again                                                                                                                                                                                                                                 |
| 51165 | 200 | The number of {instrument} reduce-only orders reached the upper limit of {upLimit}. Cancel some orders to proceed.                                                                                                                                                                                   |
| 51166 | 200 | Currently, we don't support leading trades with this instrument                                                                                                                                                                                                                                      |
| 51167 | 200 | Failed. You have block trading open order(s), please proceed after canceling existing order(s).                                                                                                                                                                                                      |
| 51168 | 200 | Failed. You have reduce-only type of open order(s), please proceed after canceling existing order(s)                                                                                                                                                                                                 |
| 51320 | 200 | The range of coin percentage is {0}%-{1}%                                                                                                                                                                                                                                                            |
| 51321 | 200 | You're leading trades. Currently, we don't support leading trades with arbitrage, iceberg, or TWAP bots                                                                                                                                                                                              |
| 51322 | 200 | You're leading trades that have been filled at market price. We've canceled your open stop orders to close your positions                                                                                                                                                                            |
| 51323 | 200 | You're already leading trades with take profit or stop loss settings. Cancel your existing stop orders to proceed                                                                                                                                                                                    |
| 51324 | 200 | As a lead trader, you hold positions in {instrument}. To close your positions, place orders in the amount that equals the available amount for closing                                                                                                                                               |
| 51325 | 200 | As a lead trader, you must use market price when placing stop orders                                                                                                                                                                                                                                 |
| 51326 | 200 | As a lead trader, you must use market price when placing orders with take profit or stop loss settings                                                                                                                                                                                               |
| 51820 | 200 | Request failed                                                                                                                                                                                                                                                                                       |
| 51821 | 200 | The payment method is not supported                                                                                                                                                                                                                                                                  |
| 51822 | 200 | Quote expired                                                                                                                                                                                                                                                                                        |
| 51823 | 200 | Parameter {param} of buy/sell trading is inconsistent with the quotation                                                                                                                                                                                                                             |
| 54000 | 200 | Margin trading is not supported.                                                                                                                                                                                                                                                                     |
| 54001 | 200 | Only Multi-currency margin account can be set to borrow coins automatically.                                                                                                                                                                                                                         |
| 54004 | 200 | Order placement or modification failed because one of the orders in the batch failed.                                                                                                                                                                                                                |
| 54005 | 200 | Switch to isolated margin mode to trade pre-market expiry futures.                                                                                                                                                                                                                                   |
| 54006 | 200 | Pre-market expiry future position limit is {posLimit}. Please cancel order or close position                                                                                                                                                                                                         |
| 54007 | 200 | Instrument {instId} is not supported                                                                                                                                                                                                                                                                 |
| 54008 | 200 | This operation is disabled by the 'mass cancel order' endpoint. Please enable it using this endpoint.                                                                                                                                                                                                |
| 54009 | 200 | The range of {param0} should be [{param1}, {param2}].                                                                                                                                                                                                                                                |
| 54011 | 200 | Pre-market trading contracts are only allowed to reduce the number of positions within 1 hour before delivery. Please modify or cancel the order.                                                                                                                                                    |
| 54012 | 200 | Due to insufficient order book depth, we are now taking measures to protect your positions. Currently, you can only cancel orders, add margin to your positions, and place post-only orders. Your positions will not be liquidated. Trade will resume once order book depth returns to a safe level. |
| 54018 | 200 | Buy limit of {param0} USD exceeded. Your remaining limit is {param1} USD. (During the call auction)                                                                                                                                                                                                  |
| 54019 | 200 | Buy limit of {param0} USD exceeded. Your remaining limit is {param1} USD. (After the call auction)                                                                                                                                                                                                   |
| 54024 | 200 | Your order failed because you must enable {ccy} as collateral to trade expiry futures, perpetual futures, and options in cross-margin mode.                                                                                                                                                          |
| 54025 | 200 | Your order failed because you must enable {ccy} as collateral to trade margin, expiry futures, perpetual futures, and options in isolated margin mode.                                                                                                                                               |
| 54026 | 200 | Your order failed because you must enable {ccy} and {ccy1} as collateral to trade the margin pair in isolated margin mode.                                                                                                                                                                           |
| 54027 | 200 | Your order failed because you must enable {ccy} as collateral to trade options.                                                                                                                                                                                                                      |
| 54028 | 200 | Your order failed because you must enable {ccy} as collateral to trade spot in isolated margin mode.                                                                                                                                                                                                 |
| 54029 | 200 | {param0} doesn’t exist within {param1}.                                                                                                                                                                                                                                                              |
| 54030 | 200 | Order failed. Your total value of same-direction {param0} open positions and orders can't exceed {param1} USD or {param2} of the platform's open interest.                                                                                                                                           |
| 54031 | 200 | Order failed. The {param1} USD open position limit for {param0} has been reached.                                                                                                                                                                                                                    |
| 54035 | 200 | Order failed. The platform has reached the collateral limit for this crypto, so you can only place reduce-only orders.                                                                                                                                                                               |
| 54036 | 200 | You can't place fill or kill orders when self-trade prevention is set to both maker and taker orders.                                                                                                                                                                                                |


#### Data class
|Error Code	|HTTP Status Code|	Error Message
| --------   | ------------- | -------------- |
|52000	|200	|No market data found.
