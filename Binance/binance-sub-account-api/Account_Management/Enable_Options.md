# Enable Options for Sub-account (For Master Account) 

## API Description​

Enable Options for Sub-account (For Master Account).

## HTTP Request​

POST `/sapi/v1/sub-account/eoptions/enable`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub user email |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
{  
  
    "email":"123@test.com",  
    "isEOptionsEnabled": true  // true or false  
}
```

