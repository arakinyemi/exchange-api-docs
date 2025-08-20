# Get Asset info

Query Spot asset information

Apply to: classic account


HTTP Request
```http
GET /v5/asset/transfer/query-asset-info

```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| accountType | true | string | Account type. SPOT |
| coin | false | string | Coin name, uppercase only |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| spot | Object |
| > status | string | account status. ACCOUNT_STATUS_NORMAL: normal, ACCOUNT_STATUS_UNSPECIFIED: banned |
| > assets | array | Object |
| >> coin | string | Coin |
| >> frozen | string | Freeze amount |
| >> free | string | Free balance |
| >> withdraw | string | Amount in withdrawing |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/transfer/query-asset-info
?accountType=SPOT&coin=ETH HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672136538042
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "spot": {
            "status": "ACCOUNT_STATUS_NORMAL",
            "assets": [
                {
                    "coin": "ETH",
                    "frozen": "0",
                    "free": "11.53485",
                    "withdraw": ""
                }
            ]
        }
    },
    "retExtinfo
": {},
    "time": 1672136539127
}
```

