# One click arrival deposit apply (for expired address deposit) 

## API Description​

Apply deposit credit for expired address (One click arrival)

## HTTP Request​

POST `/sapi/v1/capital/deposit/credit-apply`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| depositId | LONG | NO | Deposit record Id, priority use |
| txId | STRING | NO | Deposit txId, used when depositId is not specified |
| subAccountId | LONG | NO | Sub-accountId of Cloud user |
| subUserId | LONG | NO | Sub-userId of parent user |

> * Params need to be in the POST body

## Response Example​

```json
{  
    "code": "000000",  
    "message": "success",  
    "data":true,  
    "success": true  
}
```

