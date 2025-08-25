### Test connectivity​

```
{  
  "id": "922bcc6e-9de8-440d-9e84-7c80933a8d0d",  
  "method": "ping"  
}
```

Test connectivity to the WebSocket API.

**Note:**
You can use regular WebSocket ping frames to test connectivity as well,
WebSocket API will respond with pong frames as soon as possible.
`ping` request along with `time` is a safe way to test request-response handling in your application.

**Weight:**
1

**Parameters:**
NONE

**Data Source:**
Memory

**Response:**

```
{  
  "id": "922bcc6e-9de8-440d-9e84-7c80933a8d0d",  
  "status": 200,  
  "result": {},  
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