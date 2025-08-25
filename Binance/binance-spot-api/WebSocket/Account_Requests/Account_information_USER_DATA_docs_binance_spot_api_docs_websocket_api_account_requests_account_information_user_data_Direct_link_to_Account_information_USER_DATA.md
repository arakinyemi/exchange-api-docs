### Account information (USER\_DATA)​

```
{  
  "id": "605a6d20-6588-4cb9-afa0-b0ab087507ba",  
  "method": "account.status",  
  "params": {  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "83303b4a136ac1371795f465808367242685a9e3a42b22edb4d977d0696eb45c",  
    "timestamp": 1660801839480  
  }  
}
```

Query information about your account.

**Weight:**
20

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `apiKey` | STRING | YES |  |
| `omitZeroBalances` | BOOLEAN | NO | When set to `true`, emits only the non-zero balances of an account.  Default value: false |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

**Data Source:**
Memory => Database

**Response:**

```
{  
  "id": "605a6d20-6588-4cb9-afa0-b0ab087507ba",  
  "status": 200,  
  "result": {  
    "makerCommission": 15,  
    "takerCommission": 15,  
    "buyerCommission": 0,  
    "sellerCommission": 0,  
    "canTrade": true,  
    "canWithdraw": true,  
    "canDeposit": true,  
    "commissionRates": {  
      "maker": "0.00150000",  
      "taker": "0.00150000",  
      "buyer": "0.00000000",  
      "seller": "0.00000000"  
    },  
    "brokered": false,  
    "requireSelfTradePrevention": false,  
    "preventSor": false,  
    "updateTime": 1660801833000,  
    "accountType": "SPOT",  
    "balances": [  
      {  
        "asset": "BNB",  
        "free": "0.00000000",  
        "locked": "0.00000000"  
      },  
      {  
        "asset": "BTC",  
        "free": "1.3447112",  
        "locked": "0.08600000"  
      },  
      {  
        "asset": "USDT",  
        "free": "1021.21000000",  
        "locked": "0.00000000"  
      }  
    ],  
    "permissions": [  
      "SPOT"  
    ],  
    "uid": 354937868  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000  
,  
      "count": 20  
    }  
  ]  
}
```