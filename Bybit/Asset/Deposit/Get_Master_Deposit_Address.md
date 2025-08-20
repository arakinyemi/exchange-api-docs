# Get Master Deposit Address
Query the deposit address information of MASTER account.


HTTP Request
```http
GET /v5/asset/deposit/query-address
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| coin | true | string | Coin, uppercase only |
| chainType | false | string | Please use the value of >> chain from coin-info endpoint |

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
GET /v5/asset/deposit/query-address?coin=USDT&chainType=ETH HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672192792371
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "coin": "USDT",
        "chains": [
            {
                "chainType": "Ethereum (ERC20)",
                "addressDeposit": "XXXXXX",
                "tagDeposit": "",
                "chain": "ETH",
                "batchReleaseLimit": "-1",
                "contractAddress": "831ec7"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1736394811459
}
```

