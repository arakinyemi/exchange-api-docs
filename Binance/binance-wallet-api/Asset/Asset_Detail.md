# Asset Detail 

## API Description​

Fetch details of assets supported on Binance.

## HTTP Request​

GET `/sapi/v1/asset/assetDetail`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

* Please get network and other deposit or withdraw details from `GET /sapi/v1/capital/config/getall`.

## Response Example​

```json
{  
        "CTR": {  
            "minWithdrawAmount": "70.00000000", //min withdraw amount  
            "depositStatus": false,//deposit status (false if ALL of networks' are false)  
            "withdrawFee": 35, // withdraw fee  
            "withdrawStatus": true, //withdraw status (false if ALL of networks' are false)  
            "depositTip": "Delisted, Deposit Suspended" //reason  
        },  
        "SKY": {  
            "minWithdrawAmount": "0.02000000",  
            "depositStatus": true,  
            "withdrawFee": 0.01,  
            "withdrawStatus": true  
        }	  
}
```

