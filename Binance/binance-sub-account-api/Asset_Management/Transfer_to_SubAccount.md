# Transfer to Sub-account of Same Master (For Sub-account) 

## API Description​

Transfer to Sub-account of Same Master

## HTTP Request​

POST `/sapi/v1/sub-account/transfer/subToSub`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| toEmail | STRING | YES | Sub-account email |
| asset | STRING | YES |  |
| amount | DECIMAL | YES |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to open Enable Spot & Margin Trading permission for the API Key which requests this endpoint.

## Response Example​

```
{  
    "txnId":"2966662589"  
}
```

