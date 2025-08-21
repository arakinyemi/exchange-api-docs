### Get sub-account trading balance

Query detailed balance info of Trading Account of a sub-account via the master account (applies to master accounts only).

**Rate limit**: 6 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/account/subaccount/balances`

Request Example
```
GET /api/v5/account/subaccount/balances?subAcct=test1
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |

Response Example
```
{
    "code": "0",
    "data": [
        {
            "adjEq": "101.46752000000001",
            "availEq": "624719833286",
            "borrowFroz": "0",
            "details": [
                {
                    "autoLendStatus": "off",
                    "autoLendMtAmt": "0",
                    "accAvgPx": "",
                    "availBal": "101.5",
                    "availEq": "101.5",
                    "borrowFroz": "0",
                    "cashBal": "101.5",
                    "ccy": "USDT",
                    "clSpotInUseAmt": "",
                    "crossLiab": "0",
                    "collateralEnabled": false,
                    "collateralRestrict": false,
                    "colBorrAutoConversion": "0",
                    "disEq": "101.46752000000001",
                    "eq": "101.5",
                    "eqUsd": "101.46752000000001",
                    "fixedBal": "0",
                    "frozenBal": "0",
                    "imr": "",
                    "interest": "0",
                    "isoEq": "0",
                    "isoLiab": "0",
                    "isoUpl": "0",
                    "liab": "0",
                    "maxLoan": "1015.0000000000001",
                    "maxSpotInUse": "",
                    "mgnRatio": "",
                    "mmr": "",
                    "notionalLever": "",
                    "openAvgPx": "",
                    "ordFrozen": "0",
                    "rewardBal": "",
                    "smtSyncEq": "0",
                    "spotBal": "",
                    "spotCopyTradingEq": "0",
                    "spotInUseAmt": "",
                    "spotIsoBal": "0",
                    "spotUpl": "",
                    "spotUplRatio": "",
                    "stgyEq": "0",
                    "totalPnl": "",
                    "totalPnlRatio": "",
                    "twap": "0",
                    "uTime": "1663854334734",
                    "upl": "0",
                    "uplLiab": "0"
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
            "ordFroz": "0",
            "totalEq": "101.46752000000001",
            "uTime": "1739332269934",
            "upl": "0"
        }
    ],
    "msg": ""
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| uTime | String | Update time of account information, millisecond format of Unix timestamp |
| totalEq | String | The total amount of equity in USD |
| isoEq | String | Isolated margin equity in USD |
| adjEq | String | Adjusted/Effective equity in USD |
| availEq | String | Account level available equity |
| ordFroz | String | Cross margin frozen for pending orders in USD |
| imr | String | Initial margin requirement in USD |
| mmr | String | Maintenance margin requirement in USD |
| borrowFroz | String | Potential borrowing IMR of the account in USD |
| mgnRatio | String | Maintenance margin ratio in USD |
| notionalUsd | String | Notional value of positions in USD |
| notionalUsdForBorrow | String | Notional value for Borrow in USD |
| notionalUsdForSwap | String | Notional value of positions for Perpetual Futures in USD |
| notionalUsdForFutures | String | Notional value of positions for Expiry Futures in USD |
| notionalUsdForOption | String | Notional value of positions for Option in USD |
| upl | String | Cross-margin info of unrealized profit and loss at the account level in USD |
| details | Array | Detailed asset information in all currencies |
