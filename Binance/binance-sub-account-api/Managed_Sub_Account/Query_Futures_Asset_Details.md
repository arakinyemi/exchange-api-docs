# Query Managed Sub-account Futures Asset Details (For Investor Master Account) 

## API Description​

Investor can use this api to query managed sub account futures asset details

## HTTP Request​

GET `/sapi/v1/managed-subaccount/fetch-future-asset`

## Request Weight(UID)​

**60**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Managed Sub Account Email |
| accountType | STRING | NO | No input or input "USDT\_FUTURE" to get UM Futures account details. Input "COIN\_FUTURE" to get CM Futures account details. |

## Response Example​

```
{  
  "code": "200",  
  "message": "OK",  
  "snapshotVos": [  
    {  
      "type": "FUTURES",  
      "updateTime": 1672893855394,  
      "data": {  
        "assets": [  
          {  
            "asset": "USDT",  
            "marginBalance": 100,  
            "walletBalance": 120  
          }  
        ],  
        "position": [  
          {  
            "symbol": "BTCUSDT",  
            "entryPrice": 17000,  
            "markPrice": 17000,  
            "positionAmt": 0.0001  
          }  
        ]  
      }  
    }  
  ]  
}
```

