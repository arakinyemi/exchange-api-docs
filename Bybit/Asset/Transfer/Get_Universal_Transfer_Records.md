# Get Universal Transfer Records
Query universal transfer records


tip

Main acct api key or Sub acct api key are both supported

Main acct api key needs "SubMemberTransfer" permission

Sub acct api key needs "SubMemberTransferList" permission

info

If startTime and endTime are not provided, the API returns data from the past 7 days by default.

If only startTime is provided, the API returns records from startTime to startTime + 7 days.

If only endTime is provided, the API returns records from endTime - 7 days to endTime.

If both are provided, the maximum allowed range is 7 days (endTime - startTime ≤ 7 days).

HTTP Request
```http
GET /v5/asset/transfer/query-universal-transfer-list
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| transferId | false | string | UUID. Use the one you generated in createTransfer |
| coin | false | string | Coin, uppercase only |
| status | false | string | Transfer status. SUCCESS,FAILED,PENDING |
| startTime | false | integer | The start timestamp (ms) Note: the query logic is actually effective based on second level |
| endTime | false | integer | The end timestamp (ms) Note: the query logic is actually effective based on second level |
| limit | false | integer | Limit for data size per page. [1, 50]. Default: 20 |
| cursor | false | string | Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > transferId | string | Transfer ID |
| > coin | string | Transferred coin |
| > amount | string | Transferred amount |
| > fromMemberId | string | From UID |
| > toMemberId | string | TO UID |
| > fromAccountType | string | From account type |
| > toAccountType | string | To account type |
| > timestamp | string | Transfer created timestamp (ms) |
| > status | string | Transfer status |
| nextPageCursor | string | Refer to the cursor request parameter |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/transfer/query-universal-transfer-list?limit=1&cursor=eyJtaW5JRCI6MTc5NjU3OCwibWF4SUQiOjE3OTY1Nzh9 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN-TYPE: 2
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672190762800
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "list": [
            {
                "transferId": "universalTransfer_4c3cfe2f-85cb-11ed-ac09-9e37823c81cd_533285",
                "coin": "USDC",
                "amount": "1000",
                "timestamp": "1672134373000",
                "status": "SUCCESS",
                "fromAccountType": "UNIFIED",
                "toAccountType": "UNIFIED",
                "fromMemberId": "533285",
                "toMemberId": "592324"
            }
        ],
        "nextPageCursor": "eyJtaW5JRCI6MTc4OTYwNSwibWF4SUQiOjE3ODk2MDV9"
    },
    "retExtinfo
": {},
    "time": 1672190763079
}
```

