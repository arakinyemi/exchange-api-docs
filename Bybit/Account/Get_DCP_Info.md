# Get DCP info

Query the DCP configuration of the account. Before calling the interface, please make sure you have applied for the UTA account DCP configuration with your account manager

Only the configured main / sub account can query information from this API. Calling this API by an account always returns empty.

If you only request to activate Spot trading for DCP, the contract and options data will not be returned.

info

Support UTA2.0:

USDT Perpetuals, USDT Futures, USDC Perpetuals, USDC Futures, Inverse Perpetuals, Inverse Futures [DERIVATIVES]

Spot [SPOT]

Options [OPTIONS]

Support UTA1.0:

USDT Perpetuals, USDT Futures, USDC Perpetuals, USDC Futures [DERIVATIVES]

Spot [SPOT]

Options [OPTIONS]

HTTP Request
```http
GET /v5/account/query-dcp-info
```

Request Parameters
None



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| dcpinfos | array`<object>` | DCP config for each product |
| > product | string | SPOT, DERIVATIVES, OPTIONS |
| > dcpStatus | string | Disconnected-CancelAll-Prevention status: ON |
| > timeWindow | string | DCP trigger time window which user pre-set. Between [3, 300] seconds, default: 10 sec |

---

Request Example

HTTP
  
```http
GET /v5/account/query-dcp-info
HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1717065530867
X-BAPI-RECV-WINDOW: 5000
```

Response Example

// it means my account enables Spot and Deriviatvies on the backend

// Options is not enabled with DCP
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "dcpinfos": [
            {
                "product": "SPOT",
                "dcpStatus": "ON",
                "timeWindow": "10"
            },
            {
                "product": "DERIVATIVES",
                "dcpStatus": "ON",
                "timeWindow": "10"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1717065531697
}
```

