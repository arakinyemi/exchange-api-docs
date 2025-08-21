### GET / Status
Get event status of system upgrade.

**Rate Limit**: 1 request per 5 seconds  

HTTP Request
`GET /api/v5/system/status`

Request Example
```
GET /api/v5/system/status
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| state | String | No | System maintenance status |

Response Example
```json
{
    "code": "0",
    "data": [
        {
            "begin": "1672823400000",
            "end": "1672823520000",
            "href": "",
            "preOpenBegin": "",
            "scheDesc": "",
            "serviceType": "8",
            "state": "completed",
            "maintType": "1",
            "env": "1",
            "system": "unified",
            "title": "Trading account system upgrade (in batches of accounts)"
        }
    ],
    "msg": ""
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| title | String | Maintenance title |
| state | String | Maintenance status |
| begin | String | Begin time |
| end | String | End time |
| preOpenBegin | String | Pre-open time |
| href | String | Details link |
| serviceType | String | Service type |
| system | String | System |
| scheDesc | String | Rescheduled description |
| maintType | String | Maintenance type |
| env | String | Environment |
