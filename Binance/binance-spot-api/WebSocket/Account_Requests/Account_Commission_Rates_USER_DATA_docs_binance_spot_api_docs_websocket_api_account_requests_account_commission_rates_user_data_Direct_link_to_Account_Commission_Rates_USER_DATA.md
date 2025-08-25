### Account Commission Rates (USER\_DATA)​

```
{  
  "id": "d3df8a61-98ea-4fe0-8f4e-0fcea5d418b0",  
  "method": "account.commission",  
  "params": {  
    "symbol": "BTCUSDT",  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "c5a5ffb79fd4f2e10a92f895d488943a57954edf5933bde3338dfb6ea6d6eefc",  
    "timestamp": 1673923281052  
  }  
}
```

Get current account commission rates.

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | YES |  |

**Weight:**
20

**Data Source:**
Database

**Response:**

```
{  
  "id": "d3df8a61-98ea-4fe0-8f4e-0fcea5d418b0",  
  "status": 200,  
  "result": {  
    "symbol": "BTCUSDT",  
    "standardCommission": {     //Standard commission rates on trades from the order.  
      "maker": "0.00000010",  
      "taker": "0.00000020",  
      "buyer": "0.00000030",  
      "seller": "0.00000040"  
    },  
    "specialCommission": {      // Special commission rates from the order.  
      "maker": "0.01000000",  
      "taker": "0.02000000",  
      "buyer": "0.03000000",  
      "seller": "0.04000000"  
    },  
    "taxCommission": {          //Tax commission rates on trades from the order.  
      "maker": "0.00000112",  
      "taker": "0.00000114",  
      "buyer": "0.00000118",  
      "seller": "0.00000116"  
    },  
    "discount": {                //Discount on standard commissions when paying in BNB.  
      "enabledForAccount": true,  
      "enabledForSymbol": true,  
      "discountAsset": "BNB",  
      "discount": "0.75000000"   //Standard commission is reduced by this rate when paying commission in BNB.  
    }  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 20  
    }  
  ]  
}
```