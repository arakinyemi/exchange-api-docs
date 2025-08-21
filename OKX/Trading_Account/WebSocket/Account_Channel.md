### Account channel

Retrieve account information. Data will be pushed when triggered by events such as placing order, canceling order, transaction execution, etc. It will also be pushed in regular interval according to subscription granularity.

Concurrent connection to this channel will be restricted by the following rules: WebSocket connection count limit.

**URL Path**  
`/ws/v5/private` (required login)

Request Example : single

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "account",
      "ccy": "BTC"
    }
  ]
}
```

Request Example

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "account",
      "extraParams": "
        {
          \"updateInterval\": \"0\"
        }
      "
    }
  ]
}
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message<br>Provided by client. It will be returned in response message for identifying the corresponding request.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>subscribe<br>unsubscribe |
| args | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name<br>account |
| > ccy | String | No | Currency |
| > extraParams | String | No | Additional configuration |
| >> updateInterval | int | No | 0: only push due to account events<br>The data will be pushed both by events and regularly if this field is omitted or set to other values than 0.<br>The following format should be strictly obeyed when using this field.<br>"extraParams": "<br>{<br>\"updateInterval\": \"0\"<br>}<br>" |

Successful Response Example : single

```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "account",
    "ccy": "BTC"
  },
  "connId": "a4d3ae55"
}
```

Successful Response Example

```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "account"
  },
  "connId": "a4d3ae55"
}
```

Failure Response Example

```
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"account\", \"ccy\" : \"BTC\"}]}",
  "connId": "a4d3ae55"
}
```

Response parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message |
| event | String | Yes | Operation<br>subscribe<br>unsubscribe<br>error |
| arg | Object | No | Subscribed channel |
| > channel | String | Yes | Channel name<br>account |
| > ccy | String | No | Currency |
| code | String | No | Error code |
| msg | String | No | Error message |
| connId | String | Yes | WebSocket connection ID |

Push Data Example

```
{
    "arg": {
        "channel": "account",
        "uid": "44*********584"
    },
    "eventType": "snapshot",
    "curPage": 1,
    "lastPage": true,
    "data": [{
        "adjEq": "55444.12216906034",
    "availEq": "55415.624719833286",
        "borrowFroz": "0",
        "details": [{
            "availBal": "4734.371190691436",
            "availEq": "4734.371190691435",
            "borrowFroz": "0",
            "cashBal": "4750.426970691436",
            "ccy": "USDT",
            "coinUsdPrice": "0.99927",
            "crossLiab": "0",
      "collateralEnabled": false,
      "collateralRestrict": false,
      "colBorrAutoConversion": "0",
            "disEq": "4889.379316336831",
            "eq": "4892.951170691435",
            "eqUsd": "4889.379316336831",
            "smtSyncEq": "0",
            "spotCopyTradingEq": "0",
            "fixedBal": "0",
            "frozenBal": "158.57998",
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
            "maxSpotInUseAmt": "",          
            "spotIsoBal": "0",
            "stgyEq": "150",
            "twap": "0",
            "uTime": "1705564213903",
            "upl": "-7.475800000000003",
            "uplLiab": "0",
            "spotBal": "",
            "openAvgPx": "",
            "accAvgPx": "",
            "spotUpl": "",
            "spotUplRatio": "",
            "totalPnl": "",
            "totalPnlRatio": ""
        }],
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
        "totalEq": "",
        "uTime": "1705564223311",
        "upl": "0"
    }]
}
```

Push data parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| arg | Object | Successfully subscribed channel |
| > channel | String | Channel name |
| > uid | String | User Identifier |
| eventType | String | Event type:<br>snapshot: Initial and regular snapshot push<br>event_update: Event-driven update push |
| curPage | Integer | Current page number.<br>Only applicable for snapshot events. Not included in event_update events. |
| lastPage | Boolean | Whether this is the last page of pagination:<br>true<br>false<br>Only applicable for snapshot events. Not included in event_update events. |
| data | Array of objects | Subscribed data |
| > uTime | String | The latest time to get account information, millisecond format of Unix timestamp, e.g. 1597026383085 |
| > totalEq | String | The total amount of equity in USD |
| > isoEq | String | Isolated margin equity in USD<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| > adjEq | String | Adjusted / Effective equity in USD<br>The net fiat value of the assets in the account that can provide margins for spot, expiry futures, perpetual futures and options under the cross-margin mode.<br>In multi-ccy or PM mode, the asset and margin requirement will all be converted to USD value to process the order check or liquidation.<br>Due to the volatility of each currency market, our platform calculates the actual USD value of each currency based on discount rates to balance market risks.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > availEq | String | Account level available equity, excluding currencies that are restricted due to the collateralized borrowing limit.<br>Applicable to Multi-currency margin/Portfolio margin |
| > ordFroz | String | Margin frozen for pending cross orders in USD<br>Only applicable to Spot mode/Multi-currency margin |
| > imr | String | Initial margin requirement in USD<br>The sum of initial margins of all open positions and pending orders under cross-margin mode in USD.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > mmr | String | Maintenance margin requirement in USD<br>The sum of maintenance margins of all open positions and pending orders under cross-margin mode in USD.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > borrowFroz | String | Potential borrowing IMR of the account in USD<br>Only applicable to Spot mode/Multi-currency margin/Portfolio margin. It is "" for other margin modes. |
| > mgnRatio | String | Maintenance margin ratio in USD.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > notionalUsd | String | Notional value of positions in USD<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > notionalUsdForBorrow | String | Notional value for Borrow in USD<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > notionalUsdForSwap | String | Notional value of positions for Perpetual Futures in USD<br>Applicable to Multi-currency margin/Portfolio margin |
| > notionalUsdForFutures | String | Notional value of positions for Expiry Futures in USD<br>Applicable to Multi-currency margin/Portfolio margin |
| > notionalUsdForOption | String | Notional value of positions for Option in USD<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > upl | String | Cross-margin info of unrealized profit and loss at the account level in USD<br>Applicable to Multi-currency margin/Portfolio margin |
| > details | Array of objects | Detailed asset information in all currencies |
| >> ccy | String | Currency |
| >> eq | String | Equity of currency |
| >> cashBal | String | Cash Balance |
| >> uTime | String | Update time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| >> isoEq | String | Isolated margin equity of currency<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| >> availEq | String | Available equity of currency<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| >> disEq | String | Discount equity of currency in USD |
| >> fixedBal | String | Frozen balance for Dip Sniper and Peak Sniper |
| >> availBal | String | Available balance of currency |
| >> frozenBal | String | Frozen balance of currency |
| >> ordFrozen | String | Margin frozen for open orders<br>Applicable to Spot mode/Futures mode/Multi-currency margin |
| >> liab | String | Liabilities of currency<br>It is a positive value, e.g. 21625.64.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| >> upl | String | The sum of the unrealized profit & loss of all margin and derivatives positions of currency.<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| >> uplLiab | String | Liabilities due to Unrealized loss of currency<br>Applicable to Multi-currency margin/Portfolio margin |
| >> crossLiab | String | Cross Liabilities of currency<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| >> isoLiab | String | Isolated Liabilities of currency<br>Applicable to Multi-currency margin/Portfolio margin |
| >> rewardBal | String | Trial fund balance |
| >> mgnRatio | String | Cross Maintenance margin ratio of currency<br>The index for measuring the risk of a certain asset in the account.<br>Applicable to Futures mode and when there is cross position |
| >> imr | String | Cross initial margin requirement at the currency level<br>Applicable to Futures mode and when there is cross position |
| >> mmr | String | Cross maintenance margin requirement at the currency level<br>Applicable to Futures mode and when there is cross position |
| >> interest | String | Interest of currency<br>It is a positive value, e.g."9.01". Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| >> twap | String | System is forced repayment(TWAP) indicator<br>Divided into multiple levels from 0 to 5, the larger the number, the more likely the auto repayment will be triggered.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| >> maxLoan | String | Max loan of currency<br>Applicable to cross of Spot mode/Multi-currency margin/Portfolio margin |
| >> eqUsd | String | Equity USD of currency |
| >> borrowFroz | String | Potential borrowing IMR of currency in USD<br>Only applicable to Spot mode/Multi-currency margin/Portfolio margin. It is "" for other margin modes. |
| >> notionalLever | String | Leverage of currency<br>Applicable to Futures mode |
| >> coinUsdPrice | String | Price index USD of currency |
| >> stgyEq | String | strategy equity |
| >> isoUpl | String | Isolated unrealized profit and loss of currency<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| >> spotInUseAmt | String | Spot in use amount<br>Applicable to Portfolio margin |
| >> clSpotInUseAmt | String | User-defined spot risk offset amount<br>Applicable to Portfolio margin |
| >> maxSpotInUseAmt | String | Max possible spot risk offset amount<br>Applicable to Portfolio margin |
| >> spotIsoBal | String | Spot isolated balance<br>Applicable to copy trading<br>Applicable to Spot mode/Futures mode |
| >> smtSyncEq | String | Smart sync equity<br>The default is "0", only applicable to copy trader. |
| >> spotCopyTradingEq | String | Spot smart sync equity.<br>The default is "0", only applicable to copy trader. |
| >> spotBal | String | Spot balance. The unit is currency, e.g. BTC. More details |
| >> openAvgPx | String | Spot average cost price. The unit is USD. More details |
| >> accAvgPx | String | Spot accumulated cost price. The unit is USD. More details |
| >> spotUpl | String | Spot unrealized profit and loss. The unit is USD. More details |
| >> spotUplRatio | String | Spot unrealized profit and loss ratio. More details |
| >> totalPnl | String | Spot accumulated profit and loss. The unit is USD. More details |
| >> totalPnlRatio | String | Spot accumulated profit and loss ratio. More details |
| >> collateralEnabled | Boolean | true: Collateral enabled<br>false: Collateral disabled<br>Applicable to Multi-currency margin |
| >> collateralRestrict | Boolean | Platform level collateralized borrow restriction<br>true<br>false |
| >> colBorrAutoConversion | String | Indicator of forced repayment when the collateralized borrowing on a crypto reaches the platform limit and users' trading accounts hold this crypto.<br>Divided into multiple levels from 1-5, the larger the number, the more likely the repayment will be triggered.<br>The default will be 0, indicating there is no risk currently. 5 means this user is undergoing auto conversion now.<br>Applicable to Spot mode/Futures mode/Multi-currency margin/Portfolio margin |

💡 "" will be returned for inapplicable fields under the current account level.

**Important notes:**
- The account data is sent on event basis and regular basis.
- The event push is not pushed in real-time. It is aggregated and pushed at a fixed time interval, around 50ms. For example, if multiple events occur within a fixed time interval, the system will aggregate them into a single message and push it at the end of the fixed time interval. If the data volume is too large, it may be split into multiple messages.
- The regular push sends updates regardless of whether there are activities in the trading account or not.
- Only currencies with non-zero balance will be pushed. Definition of non-zero balance: any value of eq, availEq, availBql parameters is not 0. If the data is too large to be sent in a single push message, it will be split into multiple messages.
- For example, when subscribing to account channel without specifying ccy and there are 5 currencies are with non-zero balance, all 5 currencies data will be pushed in initial snapshot and in regular update. Subsequently when there is change in balance or equity of an token, only the incremental data of that currency will be pushed triggered by this change.
