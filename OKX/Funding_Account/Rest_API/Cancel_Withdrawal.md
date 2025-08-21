### Cancel Withdrawal

You can cancel normal withdrawal requests, but you cannot cancel withdrawal requests on Lightning.

**Rate Limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

HTTP Request
`POST /api/v5/asset/cancel-withdrawal`

Request Example
```
POST /api/v5/asset/cancel-withdrawal
body {
   "wdId":"1123456"
}
```

Request Parameters

| Parameter | Type   | Required | Description     |
|-----------|--------|----------|-----------------|
| wdId      | String | Yes      | Withdrawal ID   |

Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "wdId": "1123456"   
    }
  ]
}
```

Response Parameters

| Parameter | Type   | Description     |
|-----------|--------|-----------------|
| wdId      | String | Withdrawal ID   |

> Note: If the code is equal to 0, it cannot be strictly considered that the withdrawal has been revoked. It only means that your request is accepted by the server. The actual result is subject to the status in the withdrawal history.
