# Get Wallet Balance
Obtain wallet balance, query asset information of each currency. By default, currency information with assets or liabilities of 0 is not returned.


HTTP Request
```http
GET /v5/account/wallet-balance
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| accountType | true | string | Account type <br> UTA2.0: UNIFIED <br> UTA1.0: UNIFIED, CONTRACT(inverse derivatives wallet) <br> Classic account: CONTRACT, SPOT <br>To get Funding wallet balance, please | to this endpoint |
| coin | false | string | Coin name, uppercase only <br> If not passed, it returns non-zero asset info <br> You can pass multiple coins to query, separated by comma. USDT,USDC|

---




Response Parameters
| Parameter                    | Type    | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| list                         | array   | Object                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > accountType                | string  | Account type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > accountLTV                 | string  | deprecated field                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > accountIMRate              | string  | Account IM rate<br>You can refer to this Glossary to understand the below fields calculation and mearning<br>All account wide fields are not applicable to<br>UTA2.0(isolated margin),<br>UTA1.0(isolated margin), UTA1.0(CONTRACT),<br>classic account(SPOT, CONTRACT)                                                                                                                                                                                                                                                                          |
| > accountIMRateByMp          | string  | Account IM rate calculated by mark price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > accountMMRate              | string  | Account MM rate                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > accountMMRateByMp          | string  | Account MM rate calculated by mark price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > totalEquity                | string  | Account total equity (USD)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > totalWalletBalance         | string  | Account wallet balance (USD): ∑Asset Wallet Balance By USD value of each asset                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > totalMarginBalance         | string  | Account margin balance (USD): totalWalletBalance + totalPerpUPL                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > totalAvailableBalance      | string  | Account available balance (USD), Cross Margin: totalMarginBalance - totalInitialMargin                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > totalPerpUPL               | string  | Account Perps and Futures unrealised p&l (USD): ∑Each Perp and USDC Futures upl by base coin                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > totalInitialMargin         | string  | Account initial margin (USD): ∑Asset Total Initial Margin Base Coin                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > totalInitialMarginByMp     | string  | Account initial margin (USD) calculated by mark price: ∑Asset Total Initial Margin Base Coin calculated by mark price                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > totalMaintenanceMargin     | string  | Account maintenance margin (USD): ∑ Asset Total Maintenance Margin Base Coin                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > totalMaintenanceMarginByMp | string  | Account maintenance margin (USD) calculated by mark price: ∑ Asset Total Maintenance Margin Base Coin calculated by mark price                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > coin                       | array   | Object                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| >> coin                      | string  | Coin name, such as BTC, ETH, USDT, USDC                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| >> equity                    | string  | Equity of coin                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| >> usdValue                  | string  | USD value of coin                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| >> walletBalance             | string  | Wallet balance of coin                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| >> free                      | string  | Available balance for Spot wallet. This is a unique field for Classic SPOT                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| >> locked                    | string  | Locked balance due to the Spot open order                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| >> spotHedgingQty            | string  | The spot asset qty that is used to hedge in the portfolio margin, truncate to 8 decimals and "0" by default                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| >> borrowAmount              | string  | Borrow amount of current coin                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| >> availableToWithdraw       | string  | Note: this field is deprecated for accountType=UNIFIED from 9 Jan, 2025<br>Transferable balance: you can use Get Transferable Amount (Unified) or Get All Coins Balance instead<br>Derivatives available balance:<br>isolated margin: walletBalance - totalPositionIM - totalOrderIM - locked - bonus<br>cross & portfolio margin: look at field totalAvailableBalance(USD), which needs to be converted into the available balance of accordingly coin through index price<br>Spot (margin) available balance: refer to Get Borrow Quota (Spot) |
| >> accruedInterest           | string  | Accrued interest                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| >> totalOrderIM              | string  | Pre-occupied margin for order. For portfolio margin mode, it returns ""                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| >> totalPositionIM           | string  | Sum of initial margin of all positions + Pre-occupied liquidation fee. For portfolio margin mode, it returns ""                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| >> totalPositionMM           | string  | Sum of maintenance margin for all positions. For portfolio margin mode, it returns ""                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| >> unrealisedPnl             | string  | Unrealised P&L                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| >> cumRealisedPnl            | string  | Cumulative Realised P&L                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| >> bonus                     | string  | Bonus. This is a unique field for accounType=UNIFIED                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| >> marginCollateral          | boolean | Whether it can be used as a margin collateral currency (platform), true: YES, false: NO<br>When marginCollateral=false, then collateralSwitch is meaningless                                                                                                                                                                                                                                                                                                                                                                                     |
| >> collateralSwitch          | boolean | Whether the collateral is turned on by user (user), true: ON, false: OFF<br>When marginCollateral=true, then collateralSwitch is meaningful                                                                                                                                                                                                                                                                                                                                                                                                      |
| >> availableToBorrow         | string  | deprecated field, always return "". Please refer to availableToBorrow in the Get Collateral Info                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
---



Request Example

HTTP
 
  
```http
GET /v5/account/wallet-balance?accountType=UNIFIED&coin=BTC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672125440406
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "totalEquity": "3.31216591",
                "accountIMRate": "0",
                "accountIMRateByMp": "0",
                "totalMarginBalance": "3.00326056",
                "totalInitialMargin": "0",
                "totalInitialMarginByMp": "0",
                "accountType": "UNIFIED",
                "totalAvailableBalance": "3.00326056",
                "accountMMRate": "0",
                "accountMMRateByMp": "0",
                "totalPerpUPL": "0",
                "totalWalletBalance": "3.00326056",
                "accountLTV": "0",
                "totalMaintenanceMargin": "0",
                "totalMaintenanceMarginByMp": "0",
                "coin": [
                    {
                        "availableToBorrow": "3",
                        "bonus": "0",
                        "accruedInterest": "0",
                        "availableToWithdraw": "0",
                        "totalOrderIM": "0",
                        "equity": "0",
                        "totalPositionMM": "0",
                        "usdValue": "0",
                        "spotHedgingQty": "0.01592413",
                        "unrealisedPnl": "0",
                        "collateralSwitch": true,
                        "borrowAmount": "0.0",
                        "totalPositionIM": "0",
                        "walletBalance": "0",
                        "cumRealisedPnl": "0",
                        "locked": "0",
                        "marginCollateral": true,
                        "coin": "BTC"
                    }
                ]
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1690872862481
}
```

