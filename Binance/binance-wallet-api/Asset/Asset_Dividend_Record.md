# Asset Dividend Record 

## API Description​

Query asset dividend record.

## HTTP Request​

GET `/sapi/v1/asset/assetDividend`

## Request Weight(IP)​

**10**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| asset | STRING | NO |  |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| limit | INT | NO | Default 20, max 500 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * There cannot be more than 180 days between parameter `startTime` and `endTime`.

## Response Example​

```json
{  
    "rows":[  
        {  
            "id":1637366104,  
            "amount":"10.00000000",  
            "asset":"BHFT",  
            "divTime":1563189166000,  
            "enInfo":"BHFT distribution",  
            "tranId":2968885920  
        },  
        {  
            "id":1631750237,  
            "amount":"10.00000000",  
            "asset":"BHFT",  
            "divTime":1563189165000,  
            "enInfo":"BHFT distribution",  
            "tranId":2968885920  
        }  
    ],  
    "total":2  
}
```

