# Enable Futures for Sub-account (For Master Account) 

## API Description​

Enable Futures for Sub-account for Master Account

## HTTP Request​

POST `/sapi/v1/sub-account/futures/enable`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub-account email |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
{  
  
    "email":"123@test.com",  
    "isFuturesEnabled": true  // true or false  
  
}
```

