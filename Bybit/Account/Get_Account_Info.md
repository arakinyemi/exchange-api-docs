# Get Account Info
Query the account information, like margin mode, account mode, etc.

HTTP Request
```http
GET /v5/account/info
```
Request Parameters
None

Response Parameters
| Parameter           | Type    | Comments                                                     |
|---------------------|---------|--------------------------------------------------------------|
| unifiedMarginStatus | integer | Account status                                               |
| marginMode          | string  | ISOLATED_MARGIN, REGULAR_MARGIN, PORTFOLIO_MARGIN            |
| isMasterTrader      | boolean | Whether this account is a leader (copytrading). true, false  |
| spotHedgingStatus   | string  | Whether the unified account enables Spot hedging. ON, OFF    |
| updatedTime         | string  | Account data updated timestamp (ms)                          |
| dcpStatus           | string  | deprecated, always OFF. Please use Get DCP Info              |
| timeWindow          | integer | deprecated, always 0. Please use Get DCP Info                |
| smpGroup            | integer | deprecated, always 0. Please query Get SMP Group ID endpoint |


---

Request Example

```http
GET /v5/account/info HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672129307221
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "marginMode": "REGULAR_MARGIN",
        "updatedTime": "1697078946000",
        "unifiedMarginStatus": 4,
        "dcpStatus": "OFF",
        "timeWindow": 10,
        "smpGroup": 0,
        "isMasterTrader": false,
        "spotHedgingStatus": "OFF"
    }
}
```