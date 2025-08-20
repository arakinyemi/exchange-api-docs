# Get Single Coin Balance
Query the balance of a specific coin in a specific account type. Supports querying sub UID's balance. Also, you can check the transferable amount from master to sub account, sub to master account or sub to sub account, especially for user who has an institutional loan.


HTTP Request
```http
GET /v5/asset/transfer/query-account-coin-balance
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| memberId | false | string | UID. Required when querying sub UID balance with master api key |
| toMemberId | false | string | UID. Required when querying the transferable balance between different UIDs |
| accountType | true | string | Account type |
| toAccountType | false | string | To account type. Required when querying the transferable balance between different account types |
| coin | true | string | Coin, uppercase only |
| withBonus | false | integer | 0(default): not query bonus. 1: query bonus |
| withTransferSafeAmount | false | integer | Whether query delay withdraw/transfer safe amount <br> 0(default): false, 1: true <br>What is delay withdraw amount? |
| withLtvTransferSafeAmount | false | integer | For OTC loan users in particular, you can check the transferable amount under risk level <br> 0(default): false, 1: true <br> toAccountType is mandatory |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| accountType | string | Account type |
| bizType | integer | Biz type |
| accountId | string | Account ID |
| memberId | string | Uid |
| balance | Object |
| > coin | string | Coin |
| > walletBalance | string | Wallet balance |
| > transferBalance | string | Transferable balance |
| > bonus | string | bonus |
| > transferSafeAmount | string | Safe amount to transfer. Keep "" if not query |
| > ltvTransferSafeAmount | string | Transferable amount for ins loan account. Keep "" if not query |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/transfer/query-account-coin-balance?accountType=UNIFIED&coin=USDT&toAccountType=FUND&withLtvTransferSafeAmount=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: xxxxx
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1690254520644
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "accountType": "UNIFIED",
        "bizType": 1,
        "accountId": "1631385",
        "memberId": "1631373",
        "balance": {
            "coin": "USDT",
            "walletBalance": "11999",
            "transferBalance": "11999",
            "bonus": "0",
            "transferSafeAmount": "",
            "ltvTransferSafeAmount": "7602.4861"
        }
    },
    "retExtinfo
": {},
    "time": 1690254521256
}
```

