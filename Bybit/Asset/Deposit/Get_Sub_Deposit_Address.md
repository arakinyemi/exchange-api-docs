# Get Sub Deposit Address
Query the deposit address information of SUB account.

info

Use master UID's api key only
Custodial sub account deposit address cannot be obtained

HTTP Request
```http
GET /v5/asset/deposit/query-sub-member-address
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| coin | true | string | Coin, uppercase only |
| chainType | true | string | Please use the value of chain from coin-info endpoint |
| subMemberId | true | string | Sub user ID |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| coin | string | Coin |
| chains | array | Object |
| > chainType | string | Chain type |
| > addressDeposit | string | The address for deposit |
| > tagDeposit | string | Tag of deposit |
| > chain | string | Chain |
| > batchReleaseLimit | string | The deposit limit for this coin in this chain. "-1" means no limit |
| > contractAddress | string | The contract address of the coin. Only display last 6 characters, if there is no contract address, it shows "" |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/deposit/query-sub-member-address?coin=USDT&chainType=TRX&subMemberId=592334 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672194349421
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "coin": "USDT",
        "chains": {
            "chainType": "TRC20",
            "addressDeposit": "XXXXXX",
            "tagDeposit": "",
            "chain": "TRX",
            "batchReleaseLimit": "-1",
            "contractAddress": "gjLj6t"
        }
    },
    "retExtinfo
": {},
    "time": 1736394845821
}
```

