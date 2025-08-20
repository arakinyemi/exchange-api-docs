# Upgrade to Unified Account
Upgrade Guidance
Check your current account status by calling this # Get Account info


if unifiedMarginStatus=1, then it is Classic account, you can call below upgrade endpoint to UTA2.0 Pro. Check # Get Account info
 after a while and if unifiedMarginStatus=6, then it is successfully upgraded to UTA2.0 Pro.

if unifiedMarginStatus=3, then it is UTA1.0, you have to head to website to click "upgrade" to UTA2.0 first.

if unifiedMarginStatus=4, then it is UTA1.0 Pro, you can call below upgrade endpoint to UTA2.0 Pro. Check # Get Account info
 after a while and if unifiedMarginStatus=6, then it is successfully upgraded to UTA2.0 Pro.

if unifiedMarginStatus=5, then it is UTA2.0, you can call below upgrade endpoint to UTA2.0 Pro. Check # Get Account info
 after a while and if unifiedMarginStatus=6, then it is successfully upgraded to UTA2.0 Pro.

important

Banned users cannot upgrade the account to Unified Account

info

You can upgrade the normal acct to unified acct without closing positions now, but please note belows:

Please avoid upgrading during these period:

every hour	50th minute to 5th minute of next hour

Please ensure: there is no open orders when upgrade from UTA2.0 to UTA2.0 Pro

Regaring the conditions that upgrade UTA1.0 Pro to UTA2.0 Pro, please ensure:

There is no open orders regardless of order types;

All inverse contract positions must keep consistent with the margin mode of Unified account. If it is Portfolio Margin mode, you either close inverse positions or switch unified account margin mode to cross or isolated margin mode.

Cannot have hedge mode inverse futures positions, which is not supported in UTA2.0

Cannot have TPSL order either

During the account upgrade process, the data of Rest API/Websocket stream may be inaccurate due to the fact that the account-related asset data is in the processing state. It is recommended to query and use it after the upgrade is completed.

HTTP Request
```http
POST /v5/account/upgrade-to-uta
```

Request Parameters
None



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| unifiedUpdateStatus | string | Upgrade status. FAIL,PROCESS,SUCCESS |
| unifiedUpdateMsg | Object | If PROCESS,SUCCESS, it returns null |
| > msg | array | Error message array. Only FAIL will have this field |

---


Request Example

HTTP
 
  
  

  
```http
POST /v5/account/upgrade-to-uta HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672125123533
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "unifiedUpdateStatus": "FAIL",
        "unifiedUpdateMsg": {
            "msg": [
                "Update account failed. You have outstanding liabilities in your Spot account.",
                "Update account failed. Please close the usdc perpetual positions in USDC Account.",
                "unable to upgrade, please cancel the usdt perpetual open orders in USDT account.",
                "unable to upgrade, please close the usdt perpetual positions in USDT account."
            ]
    }
},
    "retExtinfo
": {},
    "time": 1672125124195
}
```

