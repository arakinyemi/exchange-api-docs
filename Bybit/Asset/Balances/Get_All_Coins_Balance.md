# Get All Coins Balance
You could get all coin balance of all account types under the master account, and sub account.


HTTP Request
```http
GET /v5/asset/transfer/query-account-coins-balance
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| memberId | false | string | User Id. It is required when you use master api key to check sub account coin balance |
| accountType | true | string | Account type |
| coin | false | string | Coin name, uppercase only <br> Query all coins if not passed <br> Can query multiple coins, separated by comma. USDT,USDC,ETH<br>Note: this field is mandatory for accountType=UNIFIED, and supports up to 10 coins each request |
| withBonus | false | integer | 0(default): not query bonus. 1: query bonus |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| accountType | string | Account type |
| memberId | string | UserID |
| balance | array | Object |
| > coin | string | Currency |
| > walletBalance | string | Wallet balance |
| > transferBalance | string | Transferable balance |
| > bonus | string | Bonus |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/transfer/query-account-coins-balance?accountType=FUND&coin=USDC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675866354698
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "memberId": "XXXX",
        "accountType": "FUND",
        "balance": [
            {
                "coin": "USDC",
                "transferBalance": "0",
                "walletBalance": "0",
                "bonus": ""
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1675866354913
}
```

