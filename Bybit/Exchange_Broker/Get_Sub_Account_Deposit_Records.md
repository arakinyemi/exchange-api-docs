# Get Sub Account Deposit Records
Exchange broker can query subaccount's deposit records by main UID's API key without specifying uid.

API rate limit: 300 req / min


tip

endTime - startTime should be less than 30 days. Queries for the last 30 days worth of records by default.


HTTP Request
```http
GET /v5/broker/asset/query-sub-member-deposit-record
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| id | false | string | Internal ID: Can be used to uniquely identify and filter the deposit. When combined with other parameters, this field takes the highest priority |
| txID | false | string | Transaction ID: Please note that data generated before Jan 1, 2024 cannot be queried using txID |
| subMemberId | false | string | Sub UID |
| coin | false | string | Coin, uppercase only |
| startTime | false | integer | The start timestamp (ms) Note: the query logic is actually effective based on second level |
| endTime | false | integer | The end timestamp (ms) Note: the query logic is actually effective based on second level |
| limit | false | integer | Limit for data size per page. [1, 50]. Default: 50 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| rows | array | Object |
| > id | string | Unique ID |
| > subMemberId | string | Sub account user ID |
| > coin | string | Coin |
| > chain | string | Chain |
| > amount | string | Amount |
| > txID | string | Transaction ID |
| > status | integer | Deposit status |
| > toAddress | string | Deposit target address |
| > tag | string | Tag of deposit target address |
| > depositFee | string | Deposit fee |
| > successAt | string | Last updated time |
| > confirmations | string | Number of confirmation blocks |
| > txIndex | string | Transaction sequence number |
| > blockHash | string | Hash number on the chain |
| > batchReleaseLimit | string | The deposit limit for this coin in this chain. "-1" means no limit |
| > depositType | string | The deposit type. 0: normal deposit, 10: the deposit reaches daily deposit limit, 20: abnormal deposit |
| > fromAddress | string | From address of deposit, only shown when the deposit comes from on-chain and from address is unique, otherwise gives "" |
| > taxDepositRecordsId | string | This field is used for tax purposes by Bybit EU (Austria) users， declare tax id |
| > taxStatus | integer | This field is used for tax purposes by Bybit EU (Austria) users <br> 0: No reporting required <br> 1: Reporting pending <br> 2: Reporting completed |
| nextPageCursor | string | Refer to the cursor request parameter |

---

Request Example

HTTP
 
  
```http
GET /v5/broker/asset/query-sub-member-deposit-record?coin=USDT&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672192441294
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "rows": [],
        "nextPageCursor": ""
    },
    "retExtinfo
": {},
    "time": 1672192441742
}
```

