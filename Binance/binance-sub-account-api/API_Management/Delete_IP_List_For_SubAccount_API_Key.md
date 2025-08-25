# Delete IP List For a Sub-account API Key (For Master Account) 

## API Description​

Delete IP List For a Sub-account API Key

## HTTP Request​

DELETE `/sapi/v1/sub-account/subAccountApi/ipRestriction/ipList`

## Request Weight(UID)​

**3000**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub-account email |
| subAccountApiKey | STRING | YES |  |
| ipAddress | STRING | YES | IPs to be deleted. Can be added in batches, separated by commas |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to enable Enable Spot & Margin Trading option for the api key which requests this endpoint

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

