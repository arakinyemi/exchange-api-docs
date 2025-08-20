# Get Withdrawal Records
Query withdrawal records.


tip

endTime - startTime should be less than 30 days. Query last 30 days records by default.

Can query by the master UID's api key only

HTTP Request
```http
GET /v5/asset/withdraw/query-record
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| withdrawID | false | string | Withdraw ID |
| txID | false | string | Transaction hash ID |
| coin | false | string | Coin, uppercase only |
| withdrawType | false | integer | Withdraw type. 0(default): on chain. 1: off chain. 2: all |
| startTime | false | integer | The start timestamp (ms) |
| endTime | false | integer | The end timestamp (ms) |
| limit | false | integer | Limit for data size per page. [1, 50]. Default: 50 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| rows | array | Object |
| > txID | string | Transaction ID. It returns "" when withdrawal failed, withdrawal cancelled |
| > coin | string | Coin |
| > chain | string | Chain |
| > amount | string | Amount |
| > withdrawFee | string | Withdraw fee |
| > status | string | Withdraw status |
| > toAddress | string | To withdrawal address. Shows the Bybit UID for internal transfers |
| > tag | string | Tag |
| > createTime | string | Withdraw created timestamp (ms) |
| > updateTime | string | Withdraw updated timestamp (ms) |
| > withdrawId | string | Withdraw ID |
| > withdrawType | integer | Withdraw type. 0: on chain. 1: off chain |
| > fee | string |
| > tax | string |
| > taxRate | string |
| > taxType | string |
| nextPageCursor | string | Cursor. Used for pagination |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/withdraw/query-record?coin=USDT&withdrawType=2&limit=2 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672194949557
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "rows": [
            {
                "coin": "USDC",
                "chain": "ETH",
                "amount": "41.43008",
                "txID": "0x3d7bddb797f0e86420c982c0723653b8b728fd0ec9953b6b354445848d83a185",
                "status": "success",
                "toAddress": "0xE3De6d711e0951d34777b5Cd93c827F822ee8514",
                "tag": "",
                "withdrawFee": "5",
                "createTime": "1742738305000",
                "updateTime": "1742738340000",
                "withdrawId": "131629076",
                "withdrawType": 0,
                "fee": "",
                "tax": "",
                "taxRate": "",
                "taxType": ""
            },
            {
                "coin": "USDT",
                "chain": "SOL",
                "amount": "951",
                "txID": "53j7mUftUboJ2TVb1q3zjwNi9gNGWyQ8xhEpkFovzqaTf8LzuZKzr83XjbG62TZWBkWbn27km7SD6Sc9e1BuWUfJ",
                "status": "success",
                "toAddress": "DhTEGye1vq2PPr8DPWit4HTDprnvnDiqpVHnHSY1Y82p",
                "tag": "",
                "withdrawFee": "1",
                "createTime": "1742729329000",
                "updateTime": "1742729437000",
                "withdrawId": "131603458",
                "withdrawType": 0,
                "fee": "",
                "tax": "",
                "taxRate": "",
                "taxType": ""
            }
        ],
        "nextPageCursor": "eyJtaW5JRCI6MTMxNjAzNDU4LCJtYXhJRCI6MTMxNjI5MDc2fQ=="
    },
    "retExtinfo
": {},
    "time": 1750777316807
}
```

