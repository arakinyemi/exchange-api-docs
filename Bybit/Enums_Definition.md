# Enums Definitions



## locale
- `de-DE`
- `en-US`
- `es-AR`
- `es-ES`
- `es-MX`
- `fr-FR`
- `kk-KZ`
- `id-ID`
- `uk-UA`
- `ja-JP`
- `ru-RU`
- `th-TH`
- `pt-BR`
- `tr-TR`
- `vi-VN`
- `zh-TW`
- `ar-SA`
- `hi-IN`
- `fil-PH`

***

## announcementType
- `new_crypto`
- `latest_bybit_news`
- `delistings`
- `latest_activities`
- `product_updates`
- `maintenance_updates`
- `new_fiat_listings`
- `other`

***

## announcementTag

- Spot
- Derivatives
- Spot Listings
- BTC
- ETH
- Trading Bots
- USDC
- Leveraged Tokens
- USDT
- Margin Trading
- Partnerships
- Launchpad
- Upgrades
- ByVotes
- Delistings
- VIP
- Futures
- Institutions
- Options
- WEB3
- Copy Trading
- Earn
- Bybit Savings
- Dual Asset
- Liquidity Mining
- Shark Fin
- Launchpool
- NFT GrabPic
- Buy Crypto
- P2P Trading
- Fiat Deposit
- Crypto Deposit

### Russian / Ukrainian / Other Translations

| English                     | Russian/Ukrainian/Other equivalents                          |
|-----------------------------|-------------------------------------------------------------|
| Spot                        | Спот                                                        |
| Spot Listings               | Спот лістинги / Спот листинги                               |
| Trading Bots                | Торгові боти / Торговые боты                                |
| Leveraged Tokens            | Токени з кредитним плечем                                   |
| Margin Trading              | Маржинальна торгівля / Маржинальная торговля               |
| Partnership                | Партнерство                                                |
| Upgrades                   | Оновлення / Обновление                                     |
| Delistings                 | Делістинги / Делистинг                                     |
| Futures                    | Ф'ючерси / Фьючерсы                                       |
| Options                    | Опціони                                                   |
| Copy Trading               | Копітрейдинг / Копитрейдинг                               |
| Bybit Savings              | Bybit Накопичення                                         |
| Dual Asset                 | Бівалютні інвестиції / Бивалютные инвестиции              |
| Liquidity Mining           | Майнінг ліквідності / Майнинг ликвидности                 |
| Buy Crypto                 | Купівля криптовалюти / Покупка криптовалюты               |
| P2P Trading                | P2P торгівля / P2P торговля                               |
| Fiat Deposit               | Фіатні депозити / Фиатные депозиты                         |
| Crypto Deposit             | Криптодепозити / Криптодепозиты                            |

***

## category
- `Unified Account`
- `spot`
- `linear`  
  USDT perpetual, USDT Futures and USDC contract, including USDC perp, USDC futures
- `inverse`  
  Inverse contract, including Inverse perp, Inverse futures
- `option`

***

## Classic Account categories
- linear (USDT perp)
- inverse (Inverse contract, including Inverse perp, Inverse futures)
- spot

***

## orderStatus

### Open Status
- `New` — order has been placed successfully
- `PartiallyFilled`
- `Untriggered` — Conditional orders are created

### Closed Status
- `Rejected`
- `PartiallyFilledCanceled` — only spot has this order status
- `Filled`
- `Cancelled` — in derivatives, orders with this status may have an executed quantity
- `Triggered` — instantaneous state for conditional orders from Untriggered to New
- `Deactivated` — UTA: Spot TP/SL order, conditional order, OCO order are cancelled before they are triggered

***

## timeinfo
- `rce`
- `GTC` — Good Till Cancel (odTillCancel)
- `IOC` — Immediate Or Cancel
- `FOK` — Fill Or Kill
- `PostOnly`

***

## features

- Exclusive Matching: Only match non-algorithmic users; no execution against orders from Open API
- Post-Only Mechanism: Act as maker orders, adding liquidity
- Lower Priority: Execute after non-RPI orders at the same price level
- Limited Access: Initially for select market makers across multiple pairs
- Order Book Updates: Excluded from API but displayed on the GUI

***

## createType
- `CreateByUser`
- `CreateByFutureSpread` — Spread order
- `CreateByAdminClosing`
- `CreateBySettle` — USDC Futures delivery; position closed due to contract delisting; recorded as trade but not order
- `CreateByStopOrder` — Futures conditional order
- `CreateByTakeProfit` — Futures take profit order
- `CreateByPartialTakeProfit` — Futures partial take profit order
- `CreateByStopLoss` — Futures stop loss order
- `CreateByPartialStopLoss`
- `CreateByTrailingStop`
- `CreateByLiq` — Laddered liquidation to reduce the required maintenance margin
- `CreateByTakeOver_PassThrough` — If position subject to liquidation, taken over by liquidation engine and closed at bankruptcy price
- `CreateByAdl_PassThrough` — Auto-Deleveraging (ADL)
- `CreateByBlock_PassThrough` — Order placed via Paradigm
- `CreateByBlockTradeMovePosition_PassThrough` — Order created by move position
- `CreateByClosing` — Close order placed via web or app position area
- `CreateByFGridBot` — Order created via grid bot (web/app)
- `CloseByFGridBot`
- `CreateByTWAP` — Order created by TWAP (web/app)
- `CreateByTVSignal` — Order created by TV webhook (web/app)
- `CreateByMmRateClose` — Mm rate close function (web/app)
- `CreateByMartingaleBot`
- `CloseByMartingaleBot`
- `CreateByIceBerg`
- `CreateByArbitrage`
- `CreateByDdh` — Option dynamic delta hedge order (web/app)

***

## execType
- `Trade`
- `AdlTrade` — Auto-Deleveraging
- `Funding` — Funding fee
- `BustTrade` — Takeover liquidation
- `Delivery` — USDC futures delivery; position closed by contract delisted
- `Settle` — Inverse futures settlement; position closed due to delisting
- `BlockTrade`
- `MovePosition`
- `FutureSpread`
- `UNKNOWN` — Used in classic account responses, not queryable

***

## orderType
- `Market`
- `Limit`
- `UNKNOWN` — Not valid request parameter (used in some responses, mainly when execType is Funding)

***

## stopOrderType
- `TakeProfit`
- `StopLoss`
- `TrailingStop`
- `Stop`
- `PartialTakeProfit`
- `PartialStopLoss`
- `tpslOrder` — spot TP/SL order
- `OcoOrder` — spot Oco order
- `MmRateClose` — On web or app can set MMR to close position
- `BidirectionalTpslOrder` — Spot bidirectional TP/SL order

***

## tickDirection
- `PlusTick` — price rise
- `ZeroPlusTick` — trade occurs at same price as previous trade, which was higher than trade preceding it
- `MinusTick` — price drop
- `ZeroMinusTick` — trade occurs at same price as previous trade, which was lower than trade preceding it

***

## interval

| Value        | Description            |
|--------------|------------------------|
| 1, 3, 5, 15, 30, 60, 120, 240, 360, 720 | minutes |
| D            | day                    |
| W            | week                   |
| M            | month                  |

***

## intervalTime
- `5min`
- `15min`
- `30min`  (minute)
- `1h`
- `4h` (hour)
- `1d` (day)

***

## positionIdx
- `0` — one-way mode position
- `1` — Buy side of hedge-mode position
- `2` — Sell side of hedge-mode position

***

## positionStatus
- `Normal`
- `Liq` — in liquidation process
- `Adl` — in auto-deleverage process

***

## rejectReason

| Code                          | Description                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| `EC_NoError`                  | No error                                                                                      |
| `EC_Others`                   | Other unspecified error                                                                       |
| `EC_UnknownMessageType`       | Unknown message type                                                                          |
| `EC_MissingClOrdID`           | Missing ClOrdID                                                                               |
| `EC_MissingClOrdID` (typo as `EC_Missin rigClOrdID` in original) | Missing ClOrdID (duplicate entry in original)                             |
| `EC_ClOrdIDOrigClOrdIDAreTheSame` | ClOrdID and OrigClOrdID are the same                                                         |
| `EC_DuplicatedClOrdID`        | Duplicate ClOrdID                                                                             |
| `EC_OrigClOrdIDDoesNotExist`  | OrigClOrdID does not exist                                                                    |
| `EC_TooLateToCancel`           | Too late to cancel                                                                            |
| `EC_UnknownOrderType`          | Unknown order type                                                                           |
| `EC_UnknownSide`               | Unknown side                                                                                  |
| `EC_UnknownTimeinfo`           | Unknown time info                                                                            |
| `EC_WronglyRouted`             | Wrongly routed                                                                               |
| `EC_MarketOrderPriceIsNotZero`| Market order price is not zero                                                              |
| `EC_LimitOrderInvalidPrice`    | Limit order invalid price                                                                    |
| `EC_NoEnoughQtyToFill`         | Not enough quantity to fill                                                                  |
| `EC_NoImmediateQtyToFill`      | No immediate quantity to fill                                                                |
| `EC_PerCancelRequest`          | Per cancel request                                                                           |
| `EC_MarketOrderCannotBePostOnly` | Market order cannot be post-only                                                             |
| `EC_PostOnlyWillTakeLiquidity`| Post-only order will take liquidity                                                         |
| `EC_CancelReplaceOrder`        | Cancel replace order                                                                         |
| `EC_InvalidSymbolStatus`       | Invalid symbol status                                                                        |
| `EC_CancelForNoFullFill`       | Cancel for no full fill                                                                      |
| `EC_BySelfMatch`               | By self match                                                                               |
| `EC_InCallAuctionStatus`       | Used for pre-market order operation, e.g., during 2nd phase of call auction                  |
| `EC_QtyCannotBeZero`           | Quantity cannot be zero                                                                      |
| `EC_MarketOrderNoSupportTIF`  | Market order does not support Time in Force                                                  |
| `EC_ReachMaxTradeNum`          | Reached max trade number                                                                     |
| `EC_InvalidPriceScale`         | Invalid price scale                                                                         |
| `EC_BitIndexInvalid`           | Bit index invalid                                                                           |
| `EC_StopBySelfMatch`           | Stopped by self-match                                                                        |
| `EC_InvalidSmpType`            | Invalid SMP type                                                                            |
| `EC_CancelByMMP`               | Canceled by MMP                                                                             |
| `EC_InvalidUserType`           | Invalid user type                                                                           |
| `EC_InvalidMirrorOid`          | Invalid mirror OID                                                                          |
| `EC_InvalidMirrorUid`          | Invalid mirror UID                                                                          |
| `EC_EcInvalidQty`              | Invalid quantity                                                                           |
| `EC_InvalidAmount`             | Invalid amount                                                                            |
| `EC_LoadOrderCancel`           | Load order cancel                                                                         |
| `EC_MarketQuoteNoSuppSell`    | Market quote no support sell                                                               |
| `EC_DisorderOrderID`           | Disorder order ID                                                                          |
| `EC_InvalidBaseValue`          | Invalid base value                                                                        |
| `EC_LoadOrderCanMatch`         | Load order can match                                                                     |
| `EC_SecurityStatusFail`        | Security status failure                                                                     |
| `EC_ReachRiskPriceLimit`       | Reach risk price limit                                                                      |
| `EC_OrderNoteExist`            | Order note exists                                                                          |
| `EC_CancelByOrderValueZero`    | Cancel by order value zero                                                                |
| `EC_CancelByMatchValueZero`    | Cancel by match value zero                                                                |
| `EC_ReachMarketPriceLimit`     | Reach market price limit                                                                   |

***

## accountType

| Category         | Types                                                  |
|------------------|--------------------------------------------------------|
| UTA2.0           | `UNIFIED` (Unified Trading Account), `FUND` (Funding Account) |
| UTA1.0           | `CONTRACT` (Inverse Derivatives Account, no USDT), `UNIFIED`, `FUND` |
| Classic account  | `SPOT`, `CONTRACT` (Derivatives with USDT), `FUND`     |

***

## transferStatus
- `SUCCESS`
- `PENDING`
- `FAILED`

***

## depositStatus
- `0` — unknown
- `1` — toBeConfirmed
- `2` — processing
- `3` — success (finalized status)
- `4` — deposit failed
- `10011` — pending to be credited to funding pool
- `10012` — credited to funding pool successfully

***

## withdrawStatus
- `SecurityCheck`
- `Pending`
- `success`
- `CancelByUser`
- `Reject`
- `Fail`
- `BlockchainConfirmed`
- `MoreinformationRequired`
- `Unknown` (rare status)

***

## triggerBy
- `LastPrice`
- `IndexPrice`
- `MarkPrice`

***

## cancelType

| Cancel Type                 | Description                                                              |
|----------------------------|--------------------------------------------------------------------------|
| CancelByUser               | Cancelled by user                                                        |
| CancelByReduceOnly         | Cancelled by reduceOnly                                                  |
| CancelByPrepareLiq         | Cancelled to attempt liquidation prevention by freeing margin           |
| CancelAllBeforeLiq         | Cancelled due to liquidation                                             |
| CancelByPrepareAdl         | Cancelled due to ADL                                                     |
| CancelAllBeforeAdl         | Cancelled due to ADL                                                     |
| CancelByAdmin              | Cancelled by admin                                                       |
| CancelBySettle             | Cancelled due to delisting contract                                     |
| CancelByTpSlTsClear        | TP/SL order cancelled when the position is cleared                      |
| CancelBySmp                | Cancelled by SMP                                                        |
| CancelByDCP                | Cancelled by DCP triggering                                             |
| CancelByRebalance          | Spread trading: order price outside limit price range                   |
| ...                       | etc.                                                                    |

***

## optionPeriod

| Asset | Periods (days)              |
|-------|----------------------------|
| BTC   | 7, 14, 21, 30, 60, 90, 180, 270 |
| ETH   | 7, 14, 21, 30, 60, 90, 180, 270 |
| SOL   | 7, 14, 21, 30, 60, 90      |

***

## dataRecordingPeriod
- `5min`
- `15min`
- `30min` (minute)
- `1h`
- `4h` (hour)
- `4d` (day)

***

## contractType
- `InversePerpetual`
- `LinearPerpetual`
- `LinearFutures` (USDT/USDC Futures)
- `InverseFutures`

***

## status
- `PreLaunch`
- `Trading`
- `Delivering`
- `Closed`

***

## curAuctionPhase

| Phase                   | Description                                                                |
|-------------------------|----------------------------------------------------------------------------|
| NotStarted              | Pre-market trading is not started                                          |
| Finished                | Pre-market trading is finished; contract will be delisted if no continuation|
| CallAuction             | Auction phase of pre-market trading                                        |
| CallAuctionNoCancel     | Auction phase, no cancel allowed                                           |
| CrossMatching           | cross matching phase; cannot create, modify, cancel orders                 |
| ContinuousTrading       | Continuous trading phase; no restrictions on orders                        |

***

## marginTrading
- `none` — no margin trading on normal or UTA account
- `both` — supported on normal and UTA accounts
- `utaOnly` — supported only on UTA account
- `normalSpotOnly` — supported only on normal account

***

## copyTrading
- `none`
- `both`
- `utaOnly`
- `normalOnly`

***

## type (uta-translog)

- `TRANSFER_IN` — Assets transferred into Unified wallet
- `TRANSFER_OUT` — Assets transferred out from Unified wallet
- `TRADE`
- `SETTLEMENT` — USDT Perp and USDC Perp funding settlement + USDC 8-hour session settlement
- `DELIVERY` — USDC Futures, Option delivery
- `LIQUIDATION`
- `ADL` — Auto-Deleveraging
- `AIRDROP`
- `BONUS` — Bonus claimed
- `BONUS_RECOLLECT` — Bonus expired
- `FEE_REFUND`
- `INTEREST` — Interest due to borrowing
- `CURRENCY_BUY`
- `CURRENCY_SELL`
- (and many more types related to loans, staking, custody, external services...)  

***

## type (contract-translog)

- Similar types as UTA translog but specific to (inverse) derivatives wallet

***

## unifiedMarginStatus

| Value | Description                          |
|-------|------------------------------------|
| 1     | Classic account                    |
| 3     | Unified trading account 1.0       |
| 4     | Unified trading account 1.0 (pro) |
| 5     | Unified trading account 2.0       |
| 6     | Unified trading account 2.0 (pro) |

***

## ltStatus (Leveraged Token Status)
- `1` — LT can be purchased and redeemed
- `2` — LT can be purchased, but not redeemed
- `3` — LT can be redeemed, but not purchased
- `4` — LT cannot be purchased nor redeemed
- `5` — Adjusting position

***

## convertAccountType

| UTA 2.0            | Value              |
|--------------------|--------------------|
| Unified Trading Acct| eb_convert_uta     |
| Funding Account     | eb_convert_funding |

| UTA 1.0            | Value               |
|--------------------|---------------------|
| Inverse Derivatives | eb_convert_inverse  |
| Unified Trading Acct| eb_convert_uta      |
| Funding Account     | eb_convert_funding  |

| Classic Account     | Value               |
|--------------------|---------------------|
| Spot Account       | eb_convert_spot      |
| Derivatives Acct   | eb_convert_contract  |
| Funding Account    | eb_convert_funding   |

***

## symbol examples

### USDT Perpetual
- `BTCUSDT`
- `ETHUSDT`

### USDT Futures

- `BTCUSDT-21FEB25`
- `ETHUSDT-14FEB25`

Types: Weekly, Bi-Weekly, Tri-Weekly, Monthly, Bi-Monthly, Quarterly, Bi-Quarterly, Tri-Quarterly

### USDC Perpetual
- `BTCPERP`
- `ETHPERP`

### USDC Futures
- `BTC-24MAR23`

### Inverse Perpetual
- `BTCUSD`
- `ETHUSD`

### Inverse Futures

- `BTCUSDH23` (H: 1st quarter; 23: year 2023)
- `BTCUSDM23`
- `BTCUSDU23`
- `BTCUSDZ23`

### Spot
- `BTCUSDT`
- `ETHUSDC`

### Option

- `BTC-13FEB25-89000-P-USDT` (USDT Option)
- `ETH-28FEB25-2800-C` (USDC Option)

***

## vipLevel
- No VIP
- VIP-1
- VIP-2
- VIP-3
- VIP-4
- VIP-5
- VIP-Supreme
- PRO-1 through PRO-6

***

## adlRankIndicator
- `0` — default value of empty position
- `1` to `5` — ranks for auto-deleveraging

***

## smpType
- `default: None`
- `CancelMaker`
- `CancelTaker`
- `CancelBoth`

***

## extraFees.feeType
- UNKNOWN
- TAX — Government tax (Indonesian site)
- CFX — Indonesian foreign exchange tax
- WHT — EU withholding tax
- GST — Indian GST tax
- VAT — ARE VAT tax

***

## extraFees.subFeeType
- UNKNOWN
- TAX_PNN — Tax fee fiat to digital (Indonesian site)
- TAX_PPH — Tax fee digital to fiat (Indonesian site)
- CFX_FIEE — CFX fee fiat to digital (Indonesian site)
- AUT_WITHHOLDING_TAX — EU withholding tax
- IND_GST — Indian GST tax
- ARE_VAT — ARE VAT tax

***

## state
- `scheduled`
- `ongoing`
- `completed`
- `canceled`

***

## serviceTypes
- `1` — Trading service
- `2` — Trading service via HTTP Request
- `3` — Trading service via websocket
- `4` — Private websocket stream
- `5` — Market data service

***

## product
- `1` — Futures
- `2` — Spot
- `3` — Option
- `4` — Spread

***

## maintainType
- `1` — Planned maintenance
- `2` — Temporary maintenance
- `3` — Incident

***

## env
- `1` — Production
- `2` — Production Demo service

***

## bizType
- `SPOT`
- `DERIVATIVES`
- `OPTIONS`

***

## msg (example messages)
- API limit updated successfully
- Requested limit exceeds maximum allowed per user
- No permission to operate these UIDs
- API cap configuration not found
- API cap configuration not found for bizType
- Requested limit would exceed institutional quota
- Spot Fee Currency Instruction

***

## Maker Fee Calculation Example for BTCUSDT

Is makerFeeRate positive?

| Condition                       | Side | Currency used      |
|--------------------------------|------|-------------------|
| TRUE                           | Buy  | Base currency (BTC)|
| TRUE                           | Sell | Quote currency (USDT)|
| FALSE and IsMakerOrder = TRUE  | Buy  | Quote currency (USDT)|
| FALSE and IsMakerOrder = TRUE  | Sell | Base currency (BTC)|
| FALSE and IsMakerOrder = FALSE | Buy  | Base currency (BTC)|
| FALSE and IsMakerOrder = FALSE | Sell | Quote currency (USDT)|
