# Get Sub-account's Status on Margin Or Futures (For Master Account) 

## API Description​

Get Sub-account's Status on Margin Or Futures

## HTTP Request​

GET `/sapi/v1/sub-account/status`

## Request Weight(IP)​

**10**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | NO | Sub-account email |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If no email sent, all sub-accounts' information will be returned.

## Response Example​

```json
[  
    {  
        "email":"123@test.com",      // user email  
        "isSubUserEnabled": true,  	 // true or false  
        "isUserActive": true, 		 // true or false  
        "insertTime": 1570791523523,  // sub account create time  
        "isMarginEnabled": true,     // true or false for margin  
        "isFutureEnabled": true,      // true or false for futures.  
        "mobile": 1570791523523   	 // user mobile number  
    }  
]
```

