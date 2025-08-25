# Query Managed Sub-account Asset Details (For Investor Master Account) 

## API Description​

Query Managed Sub-account Asset Details

## HTTP Request​

GET `/sapi/v1/managed-subaccount/asset`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
[  
  {  
     "coin": "INJ",                  
     "name": "Injective Protocol",   
     "totalBalance": "0",            
     "availableBalance": "0",        
     "inOrder": "0",                  
     "btcValue": "0"                 
  },  
  {  
     "coin": "FILDOWN",  
     "name": "FILDOWN",  
     "totalBalance": "0",  
     "availableBalance": "0",  
     "inOrder": "0",  
     "btcValue": "0"  
  }  
]
```

