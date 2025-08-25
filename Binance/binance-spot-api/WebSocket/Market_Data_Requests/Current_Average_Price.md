### Current average price​

```
{  
  "id": "ddbfb65f-9ebf-42ec-8240-8f0f91de0867",  
  "method": "avgPrice",  
  "params": {  
    "symbol": "BNBBTC"  
  }  
}
```

Get current average price for a symbol.

**Weight:**
2

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |

**Data Source:**
Memory

**Response:**

```
{  
  "id": "ddbfb65f-9ebf-42ec-8240-8f0f91de0867",  
  "status": 200,  
  "result": {  
    "mins": 5,                    // Average price interval (in minutes)  
    "price": "9.35751834",        // Average price  
    "closeTime": 1694061154503    // Last trade time  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 2  
    }  
  ]  
}
```