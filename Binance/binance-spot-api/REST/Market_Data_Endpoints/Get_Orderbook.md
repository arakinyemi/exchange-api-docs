### Order book​

```
GET /api/v3/depth
```

**Weight:**
Adjusted based on the limit:

| Limit | Request Weight |
| --- | --- |
| 1-100 | 5 |
| 101-500 | 25 |
| 501-1000 | 50 |
| 1001-5000 | 250 |

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| symbol | STRING | YES |  |
| limit | INT | NO | Default: 100; Maximum: 5000.   If limit > 5000, only 5000 entries will be returned. |

**Data Source:**
Memory

**Response:**

```json
{  
  "lastUpdateId": 1027024,  
  "bids": [  
    [  
      "4.00000000",     // PRICE  
      "431.00000000"    // QTY  
    ]  
  ],  
  "asks": [  
    [  
      "4.00000200",  
      "12.00000000"  
    ]  
  ]  
}
```