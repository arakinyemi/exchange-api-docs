### Get / Account rate limit

Get account rate limit related information.

Only new order requests and amendment order requests will be counted towards this limit. For batch order requests consisting of multiple orders, each order will be counted individually.

Rate Limit: 1 request per second  
Rate limit rule: User ID  

HTTP Request  

`GET /api/v5/trade/account-rate-limit`

Request Example  
`GET /api/v5/trade/account-rate-limit`

Request Parameters

_None_

Response Example  

```
{
   "code":"0",
   "data":[
      {
         "accRateLimit":"2000",
         "fillRatio":"0.1234",
         "mainFillRatio":"0.1234",
         "nextAccRateLimit":"2000",
         "ts":"123456789000"
      }
   ],
   "msg":""
}
```

Response Parameters

| Parameter        | Type   | Description                                                             |
|------------------|--------|-------------------------------------------------------------------------|
| fillRatio        | String | Sub account fill ratio during the monitoring period (VIP 5+)             |
| mainFillRatio    | String | Master account aggregated fill ratio during the monitoring period (VIP 5+)|
| accRateLimit     | String | Current sub-account rate limit per two seconds                           |
| nextAccRateLimit | String | Expected sub-account rate limit per two seconds in the next period (VIP 5+)|
| ts               | String | Data update time                                                         |

***
