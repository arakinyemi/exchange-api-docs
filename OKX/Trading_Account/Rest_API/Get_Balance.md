### Get balance
Retrieve a list of assets (with non-zero balance), remaining balance, and available amount in the trading account.

Rate Limit: 10 requests per 2 seconds
Rate limit rule: User ID
Permission: Read
HTTP Request

```
GET /api/v5/account/balance
```

Request Example
```
# Get the balance of all assets in the account
GET /api/v5/account/balance

# Get the balance of BTC and ETH assets in the account
GET /api/v5/account/balance?ccy=BTC,ETH
```
Request Parameters
| Parameter | Type   | Required | Description                                                                 |
|-----------|--------|---------|-----------------------------------------------------------------------------|
| ccy       | String | No      | Single currency or multiple currencies (no more than 20) separated with comma, e.g. BTC or BTC,ETH |


Response Example

```json
{
    "code": "0",
    "data": [
        {
            "adjEq": "55415.624719833286",
            "availEq": "624719833286",
            "borrowFroz": "0",
            "details": [
                {
                    "autoLendStatus": "off",
                    "autoLendMtAmt": "0",
                    "availBal": "4834.317093622894",
                    "availEq": "4834.3170936228935",
                    "borrowFroz": "0",
                    "cashBal": "4850.435693622894",
                    "ccy": "USDT",
                    "crossLiab": "0",
                    "collateralEnabled": false,
                    "collateralRestrict": false,
                    "colBorrAutoConversion": "0",
                    "disEq": "4991.542013297616",
                    "eq": "4992.890093622894",
                    "eqUsd": "4991.542013297616",
                    "smtSyncEq": "0",
                    "spotCopyTradingEq": "0",
                    "fixedBal": "0",
                    "frozenBal": "158.573",
                    "imr": "",
                    "interest": "0",
                    "isoEq": "0",
                    "isoLiab": "0",
                    "isoUpl": "0",
                    "liab": "0",
                    "maxLoan": "0",
                    "mgnRatio": "",
                    "mmr": "",
                    "notionalLever": "",
                    "ordFrozen": "0",
                    "rewardBal": "0",
                    "spotInUseAmt": "",
                    "clSpotInUseAmt": "",
                    "maxSpotInUse": "",
                    "spotIsoBal": "0",
                    "stgyEq": "150",
                    "twap": "0",
                    "uTime": "1705449605015",
                    "upl": "-7.545600000000006",
                    "uplLiab": "0",
                    "spotBal": "",
                    "openAvgPx": "",
                    "accAvgPx": "",
                    "spotUpl": "",
                    "spotUplRatio": "",
                    "totalPnl": "",
                    "totalPnlRatio": ""
                }
            ],
            "imr": "0",
            "isoEq": "0",
            "mgnRatio": "",
            "mmr": "0",
            "notionalUsd": "0",
            "notionalUsdForBorrow": "0",
            "notionalUsdForFutures": "0",
            "notionalUsdForOption": "0",
            "notionalUsdForSwap": "0",
            "ordFroz": "",
            "totalEq": "55837.43556134779",
            "uTime": "1705474164160",
            "upl": "0",
        }
    ],
    "msg": ""
}
```

| Parameters | Types | Description |
|------------|-------|-------------|
| uTime | String | Update time of account information, millisecond format of Unix timestamp, e.g. 1597026383085 |
| totalEq | String | The total amount of equity in USD |
| isoEq | String | Isolated margin equity in USD. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| adjEq | String | Adjusted / Effective equity in USD. The net fiat value of the assets in the account that can provide margins for spot, expiry futures, perpetual futures and options under the cross-margin mode. In multi-ccy or PM mode, the asset and margin requirement will all be converted to USD value to process the order check or liquidation. Due to the volatility of each currency market, our platform calculates the actual USD value of each currency based on discount rates to balance market risks. Applicable to Multi-currency margin and Portfolio margin |
| availEq | String | Account level available equity, excluding currencies that are restricted due to the collateralized borrowing limit. Applicable to Multi-currency margin / Portfolio margin |
| ordFroz | String | Cross margin frozen for pending orders in USD. Only applicable to Multi-currency margin |
| imr | String | Initial margin requirement in USD. The sum of initial margins of all open positions and pending orders under cross-margin mode in USD. Applicable to Multi-currency margin / Portfolio margin |
| mmr | String | Maintenance margin requirement in USD. The sum of maintenance margins of all open positions and pending orders under cross-margin mode in USD. Applicable to Multi-currency margin / Portfolio margin |
| borrowFroz | String | Potential borrowing IMR of the account in USD. Only applicable to Multi-currency margin / Portfolio margin. It is "" for other margin modes. |
| mgnRatio | String | Maintenance margin ratio in USD. Applicable to Multi-currency margin / Portfolio margin |
| notionalUsd | String | Notional value of positions in USD. Applicable to Multi-currency margin / Portfolio margin |
| notionalUsdForBorrow | String | Notional value for Borrow in USD. Applicable to Spot mode / Multi-currency margin / Portfolio margin |
| notionalUsdForSwap | String | Notional value of positions for Perpetual Futures in USD. Applicable to Multi-currency margin / Portfolio margin |
| notionalUsdForFutures | String | Notional value of positions for Expiry Futures in USD. Applicable to Multi-currency margin / Portfolio margin |
| notionalUsdForOption | String | Notional value of positions for Option in USD. Applicable to Spot mode / Multi-currency margin / Portfolio margin |
| upl | String | Cross-margin info of unrealized profit and loss at the account level in USD. Applicable to Multi-currency margin / Portfolio margin |
| details | Array of objects | Detailed asset information in all currencies |
| > ccy | String | Currency |
| > eq | String | Equity of currency |
| > cashBal | String | Cash balance |
| > uTime | String | Update time of currency balance information, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| > isoEq | String | Isolated margin equity of currency. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| > availEq | String | Available equity of currency. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| > disEq | String | Discount equity of currency in USD. Applicable to Spot mode (enabled spot borrow) / Multi-currency margin / Portfolio margin |
| > fixedBal | String | Frozen balance for Dip Sniper and Peak Sniper |
| > availBal | String | Available balance of currency |
| > frozenBal | String | Frozen balance of currency |
| > ordFrozen | String | Margin frozen for open orders. Applicable to Spot mode / Futures mode / Multi-currency margin |
| > liab | String | Liabilities of currency. Positive value, e.g. 21625.64. Applicable to Multi-currency margin / Portfolio margin |
| > upl | String | Sum of unrealized profit & loss of all margin and derivatives positions of currency. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| > uplLiab | String | Liabilities due to Unrealized loss of currency. Applicable to Multi-currency margin / Portfolio margin |
| > crossLiab | String | Cross liabilities of currency. Applicable to Multi-currency margin / Portfolio margin |
| > isoLiab | String | Isolated liabilities of currency. Applicable to Multi-currency margin / Portfolio margin |
| > mgnRatio | String | Cross maintenance margin ratio of currency. Index for measuring risk of a certain asset in the account. Applicable to Futures mode and when there is cross position |
| > imr | String | Cross initial margin requirement at the currency level. Applicable to Futures mode and when there is cross position |
| > mmr | String | Cross maintenance margin requirement at the currency level. Applicable to Futures mode and when there is cross position |
| > interest | String | Accrued interest of currency. Positive value, e.g. 9.01. Applicable to Multi-currency margin / Portfolio margin |
| > twap | String | Risk indicator of auto liability repayment. Levels 0–5, higher means higher chance of auto repayment trigger. Applicable to Multi-currency margin / Portfolio margin |
| > maxLoan | String | Max loan of currency. Applicable to cross of Spot mode / Multi-currency margin / Portfolio margin |
| > eqUsd | String | Equity in USD of currency |
| > borrowFroz | String | Potential borrowing IMR of currency in USD. Applicable to Multi-currency margin / Portfolio margin; "" for other margin modes |
| > notionalLever | String | Leverage of currency. Applicable to Futures mode |
| > stgyEq | String | Strategy equity |
| > isoUpl | String | Isolated unrealized profit and loss of currency. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| > spotInUseAmt | String | Spot in use amount. Applicable to Portfolio margin |
| > spotIsoBal | String | Spot isolated balance. Applicable to copy trading and to Spot mode / Futures mode |
| > spotBal | String | Spot balance. Unit is currency, e.g. BTC |
| > openAvgPx | String | Spot average cost price. Unit is USD |
| > accAvgPx | String | Spot accumulated cost price. Unit is USD |
| > spotUpl | String | Spot unrealized profit and loss. Unit is USD |
| > spotUplRatio | String | Spot unrealized profit and loss ratio |
| > totalPnl | String | Spot accumulated profit and loss. Unit is USD |
| > totalPnlRatio | String | Spot accumulated profit and loss ratio |
| > collateralEnabled | Boolean | true: Collateral enabled; false: Collateral disabled. Applicable to Multi-currency margin |
| > collateralRestrict | Boolean | Platform-level collateralized borrow restriction (true/false) |
| > colBorrAutoConversion | String | Indicator of forced repayment when collateralized borrowing on a crypto reaches platform limit. Levels 1–5; default 0 means no risk, 5 means undergoing auto conversion. Applicable to Spot mode / Futures mode / Multi-currency margin / Portfolio margin |
| > autoLendStatus | String | Auto lend status: unsupported, off, pending, active |
| > autoLendMtAmt | String | Auto lend matched amount. Returns "0" unless active |

"" will be returned for inapplicable fields under the current account level.
 The currency details will not be returned when cashBal and eq is both 0.

 Distribution of applicable fields under each account level are as follows:

 | Parameter                 | Spot mode | Futures mode | Multi-currency margin mode | Portfolio margin mode |
| ------------------------- | --------- | ------------ | -------------------------- | --------------------- |
| **uTime**                 | Yes       | Yes          | Yes                        | Yes                   |
| **totalEq**               | Yes       | Yes          | Yes                        | Yes                   |
| **isoEq**                 |           | Yes          | Yes                        | Yes                   |
| **adjEq**                 |           |              | Yes                        | Yes                   |
| **availEq**               |           |              | Yes                        | Yes                   |
| **ordFroz**               |           |              | Yes                        | Yes                   |
| **imr**                   |           |              | Yes                        | Yes                   |
| **mmr**                   |           |              | Yes                        | Yes                   |
| **mgnRatio**              |           |              | Yes                        | Yes                   |
| **notionalUsd**           |           |              | Yes                        | Yes                   |
| **notionalUsdForSwap**    |           |              | Yes                        | Yes                   |
| **notionalUsdForFutures** |           |              | Yes                        | Yes                   |
| **notionalUsdForOption**  | Yes       |              | Yes                        | Yes                   |
| **notionalUsdForBorrow**  | Yes       |              | Yes                        | Yes                   |
| **upl**                   |           |              | Yes                        | Yes                   |
| **details**               |           |              | Yes                        | Yes                   |
| **ccy**                   | Yes       | Yes          | Yes                        | Yes                   |
| **eq**                    | Yes       | Yes          | Yes                        | Yes                   |
| **cashBal**               | Yes       | Yes          | Yes                        | Yes                   |
| **uTime (details)**       | Yes       | Yes          | Yes                        | Yes                   |
| **isoEq (details)**       |           | Yes          | Yes                        | Yes                   |
| **availEq (details)**     |           | Yes          | Yes                        | Yes                   |
| **disEq**                 | Yes       |              | Yes                        | Yes                   |
| **availBal**              | Yes       | Yes          | Yes                        | Yes                   |
| **frozenBal**             | Yes       | Yes          | Yes                        | Yes                   |
| **ordFrozen**             | Yes       | Yes          | Yes                        | Yes                   |
| **liab**                  |           |              | Yes                        | Yes                   |
| **upl (details)**         |           | Yes          | Yes                        | Yes                   |
| **uplLiab**               |           |              | Yes                        | Yes                   |
| **crossLiab**             |           |              | Yes                        | Yes                   |
| **isoLiab**               |           |              | Yes                        | Yes                   |
| **mgnRatio (details)**    |           | Yes          |                            |                       |
| **interest**              |           |              | Yes                        | Yes                   |
| **twap**                  |           |              | Yes                        | Yes                   |
| **maxLoan**               | Yes       |              | Yes                        | Yes                   |
| **eqUsd**                 | Yes       | Yes          | Yes                        | Yes                   |
| **notionalLever**         |           | Yes          |                            |                       |
| **stgyEq**                | Yes       | Yes          | Yes                        | Yes                   |
| **isoUpl**                |           | Yes          | Yes                        | Yes                   |
| **spotInUseAmt**          |           |              |                            | Yes                   |
| **spotIsoBal**            | Yes       | Yes          |                            |                       |
| **imr (details)**         |           | Yes          |                            |                       |
| **mmr (details)**         |           | Yes          |                            |                       |
| **spotBal**               | Yes       | Yes          | Yes                        | Yes                   |
| **openAvgPx**             | Yes       | Yes          | Yes                        | Yes                   |
| **accAvgPx**              | Yes       | Yes          | Yes                        | Yes                   |
| **spotUpl**               | Yes       | Yes          | Yes                        | Yes                   |
| **spotUplRatio**          | Yes       | Yes          | Yes                        | Yes                   |
| **totalPnl**              | Yes       | Yes          | Yes                        | Yes                   |
| **totalPnlRatio**         | Yes       | Yes          | Yes                        | Yes                   |
