### Get system time
Retrieve API server time.

**Rate Limit:** 10 requests per 2 seconds  
**Rate limit rule:** IP

HTTP Request
```
GET /api/v5/public/time
```

Request Example
```
GET /api/v5/public/time
```

Response Example
```
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "ts":"1597026383085"
    }
  ]
}
```

Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ts | String | System time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
