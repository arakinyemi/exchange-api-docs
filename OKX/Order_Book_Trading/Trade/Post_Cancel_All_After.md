### POST / Cancel All After

Cancel all pending orders after the countdown timeout. Applicable to all trading symbols through order book (except Spread trading)... Rate Limit: 1 request per second  
Rate limit rule: User ID + tag  
Permission: Trade  

HTTP Request  

`POST /api/v5/trade/cancel-all-after`

Request Example  
```
POST /api/v5/trade/cancel-all-after
{
   "timeOut":"60"
}
```

Request Parameters

| Parameter | Type   | Required | Description                                          |
|-----------|--------|----------|------------------------------------------------------|
| timeOut   | String | Yes      | The countdown for order cancellation, in seconds (Range: 0, ). Setting timeOut to 0 disables Cancel All After. |
| tag       | String | No       | CAA order tag (up to 16 characters)                  |

Response Example  

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "triggerTime":"1587971460",
            "tag":"",
            "ts":"1587971400"
        }
    ]
}
```

Response Parameters

| Parameter    | Type   | Description                          |
|--------------|--------|--------------------------------------|
| triggerTime  | String | The time the cancellation is triggered (0 means disabled)|
| tag          | String | CAA order tag                        |
| ts           | String | Time request is received             |

***
