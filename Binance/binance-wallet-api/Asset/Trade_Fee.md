# Trade Fee 

## API Description​

Fetch trade fee

## HTTP Request​

GET `/sapi/v1/asset/tradeFee`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| symbol | STRING | NO |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
[  
    {  
        "symbol": "ADABNB",  
        "makerCommission": "0.001",  
        "takerCommission": "0.001"  
    },  
    {  
        "symbol": "BNBBTC",  
        "makerCommission": "0.001",  
        "takerCommission": "0.001"  
    }  
]
```

