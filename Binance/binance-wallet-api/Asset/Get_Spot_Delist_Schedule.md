# Get Spot Delist Schedule 

## API Description​

Get symbols delist schedule for spot

## HTTP Request​

GET `/sapi/v1/spot/delist-schedule`

## Request Weight(IP)​

**100**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
[  
  {  
    "delistTime": 1686161202000,  
    "symbols": [  
      "BTCUSDT",   
      "ETHUSDT"  
    ]  
  },  
  {  
    "delistTime": 1686222232000,  
    "symbols": [  
      "ADAUSDT"  
    ]  
  }  
]
```

