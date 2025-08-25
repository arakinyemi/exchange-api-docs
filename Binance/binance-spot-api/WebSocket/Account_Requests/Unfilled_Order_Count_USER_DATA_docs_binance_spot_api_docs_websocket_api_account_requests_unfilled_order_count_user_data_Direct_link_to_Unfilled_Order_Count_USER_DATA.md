### Unfilled Order Count (USER\_DATA)​

```
{  
  "id": "d3783d8d-f8d1-4d2c-b8a0-b7596af5a664",  
  "method": "account.rateLimits.orders",  
  "params": {  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "76289424d6e288f4dc47d167ac824e859dabf78736f4348abbbac848d719eb94",  
    "timestamp": 1660801839500  
  }  
}
```

Query your current unfilled order count for all intervals.

**Weight:**
40

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

**Data Source:**
Memory

**Response:**

```
{  
  "id": "d3783d8d-f8d1-4d2c-b8a0-b7596af5a664",  
  "status": 200,  
  "result": [  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "SECOND",  
      "intervalNum": 10,  
      "limit": 50,  
      "count": 0  
    },  
    {  
      "rateLimitType": "ORDERS",  
      "interval": "DAY",  
      "intervalNum": 1,  
      "limit": 160000,  
      "count": 0  
    }  
  ],  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 40  
    }  
  ]  
}
```