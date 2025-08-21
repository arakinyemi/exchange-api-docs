### Asset bills history
Query the billing records of all time since 1 February, 2021.

**⚠️ IMPORTANT:** Data updates occur every 30 seconds. Update frequency may vary based on data volume - please be aware of potential delays during high-traffic periods.

**Rate Limit:** 1 Requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

HTTP Request
```
GET /api/v5/asset/bills-history
```

Request Example
```
GET /api/v5/asset/bills-history
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | No | Currency |
| type | String | No | Bill type<br>[Same extensive list as Asset bills details above] |
| clientId | String | No | Client-supplied ID for transfer or withdrawal<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| after | String | No | Pagination of data to return records earlier than the requested ts or billId, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| before | String | No | Pagination of data to return records newer than the requested ts or billId, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| limit | String | No | Number of results per request. The maximum is 100. The default is 100. |
| pagingType | String | No | PagingType<br>1: Timestamp of the bill record<br>2: Bill ID of the bill record<br>The default is 1 |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "billId": "12344",
        "ccy": "BTC",
        "clientId": "",
        "balChg": "2",
        "bal": "12",
        "type": "1",
        "ts": "1597026383085",
        "notes": ""
    }]
}
```

Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| billId | String | Bill ID |
| ccy | String | Account balance currency |
| clientId | String | Client-supplied ID for transfer or withdrawal |
| balChg | String | Change in balance at the account level |
| bal | String | Balance at the account level |
| type | String | Bill type |
| notes | String | Notes |
| ts | String | Creation time, Unix timestamp format in milliseconds, e.g.1597026383085 |
