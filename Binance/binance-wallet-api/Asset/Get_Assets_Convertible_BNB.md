# Get Assets That Can Be Converted Into BNB 

## API Description​

Get Assets That Can Be Converted Into BNB

## HTTP Request​

POST `/sapi/v1/asset/dust-btc`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| accountType | STRING | NO | `SPOT` or `MARGIN`,default `SPOT` |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
{  
    "details": [  
        {  
            "asset": "ADA",           
            "assetFullName": "ADA",   
            "amountFree": "6.21",   //Convertible amount  
            "toBTC": "0.00016848",  //BTC amount  
            "toBNB": "0.01777302",  //BNB amount（Not deducted commission fee）  
            "toBNBOffExchange": "0.01741756", //BNB amount（Deducted commission fee）  
            "exchange": "0.00035546" //Commission fee  
        }  
    ],  
    "totalTransferBtc": "0.00016848",  
    "totalTransferBNB": "0.01777302",  
    "dribbletPercentage": "0.02"     //Commission fee  
}
```

