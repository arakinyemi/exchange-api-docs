# Query Sub-account Assets (For Master Account) 

## API Description​

Fetch sub-account assets

## HTTP Request​

GET `/sapi/v4/sub-account/assets`

## Request Weight(UID)​

**60**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub Account Email |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
    "balances":[  
        {  
            "freeze":"0",  
            "withdrawing":"0",  
            "asset":"ADA",  
            "free":"10000",  
            "locked":"0"  
        },  
        {  
            "freeze":"0",  
            "withdrawing":"0",  
            "asset":"BNB",  
            "free":"10003",  
            "locked":"0"  
        },  
        {  
            "freeze":"0",  
            "withdrawing":"0",  
            "asset":"BTC",  
            "free":"11467.6399",  
            "locked":"0"  
        }  
    ]  
}
```

