# Get IP Restriction for a Sub-account API Key (For Master Account) 

## API Description​

Get IP Restriction for a Sub-account API Key

## HTTP Request​

GET `/sapi/v1/sub-account/subAccountApi/ipRestriction`

## Request Weight(UID)​

**3000**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub-account email |
| subAccountApiKey | STRING | YES |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
{  
    "ipRestrict": "true",  
    "ipList": [  
        "69.210.67.14",  
        "8.34.21.10"  
    ],  
    "updateTime": 1636371437000,  
    "apiKey": "k5V49ldtn4tszj6W3hystegdfvmGbqDzjmkCtpTvC0G74WhK7yd4rfCTo4lShf"  
}
```

