### Check server time​

```
{  
  "id": "187d3cb2-942d-484c-8271-4e2141bbadb1",  
  "method": "time"  
}
```

Test connectivity to the WebSocket API and get the current server time.

**Weight:**
1

**Parameters:**
NONE

**Data Source:**
Memory

**Response:**

```
{  
  "id": "187d3cb2-942d-484c-8271-4e2141bbadb1",  
  "status": 200,  
  "result": {  
    "serverTime": 1656400526260  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 1  
    }  
  ]  
}
```