# Get Summary of Sub-account's Margin Account (For Master Account) 

## API Description​

Get Summary of Sub-account's Margin Account

## HTTP Request​

GET `/sapi/v1/sub-account/margin/accountSummary`

## Request Weight(IP)​

**10**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
    "totalAssetOfBtc": "4.33333333",   
    "totalLiabilityOfBtc": "2.11111112",   
    "totalNetAssetOfBtc": "2.22222221",  
    "subAccountList":[  
        {  
            "email":"123@test.com",  
            "totalAssetOfBtc": "2.11111111",  
            "totalLiabilityOfBtc": "1.11111111",  
            "totalNetAssetOfBtc": "1.00000000"  
        },  
        {   
            "email":"345@test.com",  
            "totalAssetOfBtc": "2.22222222",   
            "totalLiabilityOfBtc": "1.00000001",   
            "totalNetAssetOfBtc": "1.22222221"  
        }  
    ]  
}
```

