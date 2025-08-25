# Dust Transfer 

## API Description​

Convert dust assets to BNB.

## HTTP Request​

POST `/sapi/v1/asset/dust`

## Request Weight(UID)​

**10**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| asset | ARRAY | YES | The asset being converted. For example: asset=BTC,USDT |
| accountType | STRING | NO | `SPOT` or `MARGIN`,default `SPOT` |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to open`Enable Spot & Margin Trading` permission for the API Key which requests this endpoint.

## Response Example​

```json
{  
    "totalServiceCharge":"0.02102542",  
    "totalTransfered":"1.05127099",  
    "transferResult":[  
        {  
            "amount":"0.03000000",  
            "fromAsset":"ETH",  
            "operateTime":1563368549307,  
            "serviceChargeAmount":"0.00500000",  
            "tranId":2970932918,  
            "transferedAmount":"0.25000000"  
        },  
        {  
            "amount":"0.09000000",  
            "fromAsset":"LTC",  
            "operateTime":1563368549404,  
            "serviceChargeAmount":"0.01548000",  
            "tranId":2970932918,  
            "transferedAmount":"0.77400000"  
        },  
        {  
            "amount":"248.61878453",  
            "fromAsset":"TRX",  
            "operateTime":1563368549489,  
            "serviceChargeAmount":"0.00054542",  
            "tranId":2970932918,  
            "transferedAmount":"0.02727099"  
        }  
    ]  
}
```

