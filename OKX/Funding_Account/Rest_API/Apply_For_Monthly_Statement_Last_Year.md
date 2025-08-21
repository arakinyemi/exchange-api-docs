### Apply for Monthly Statement (Last Year)

Apply for monthly statement in the past year.

**Rate Limit**: 20 requests per month  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`POST /api/v5/asset/monthly-statement`

Request Example

```
POST /api/v5/asset/monthly-statement
body
{
    "month":"Jan"
}
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| month | String | No | Month, last month by default. Valid values: Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec |

Response Example
```
{
    "code": "0",
    "data": [
        {
            "ts": "1646892328000"
        }
    ],
    "msg": ""
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ts | String | Download link generation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
