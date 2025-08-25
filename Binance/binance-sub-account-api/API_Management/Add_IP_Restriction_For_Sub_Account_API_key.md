# Add IP Restriction for Sub-Account API key (For Master Account) 

## API Description​

Add IP Restriction for Sub-Account API key

## HTTP Request​

POST `/sapi/v2/sub-account/subAccountApi/ipRestriction`

## Request Weight(UID)​

**3000**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub-account email |
| subAccountApiKey | STRING | YES |  |
| status | STRING | YES | IP Restriction status. 1 = IP Unrestricted. 2 = Restrict access to trusted IPs only. |
| ipAddress | STRING | NO | Insert static IP in batch, separated by commas. |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to enable Enable Spot & Margin Trading option for the api key which requests this endpoint

## Response Example​

```
{  
  "status": "2",   
  "ipList": [  
    "69.210.67.14",  
    "8.34.21.10",  //only return if you open IP restriction and input IP address.  
  ],  
  "updateTime": 1636371437000,  
  "apiKey": "k5V49ldtn4tszj6W3hystegdfvmGbqDzjmkCtpTvC0G74WhK7yd4rfCTo4lShf"  
}
```

