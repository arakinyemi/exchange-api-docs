# Transfer to Master (For Sub-account) 

## API Description​

Transfer to Master

## HTTP Request​

POST `/sapi/v1/sub-account/transfer/subToMaster`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
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

