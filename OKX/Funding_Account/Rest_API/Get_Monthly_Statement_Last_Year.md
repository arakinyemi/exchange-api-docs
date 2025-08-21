### Get Monthly Statement (Last Year)

Retrieve monthly statement in the past year.

**Rate Limit**: 10 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/asset/monthly-statement`

Request Example
```
GET /api/v5/asset/monthly-statement?month=Jan
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| month | String | Yes | Month, valid values: Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec |

Response Example
```
{
    "code": "0",
    "data": [
        {
            "fileHref": "http://xxx",
            "state": "finished",
            "ts": 1646892328000
        }
    ],
    "msg": ""
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| fileHref | String | Download file link |
| ts | Int | Download link generation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| state | String | Download link status: "finished", "ongoing" |
