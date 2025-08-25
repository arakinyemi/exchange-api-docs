# Get Futures Position-Risk of Sub-account (For Master Account) 

## API Description​

Get Futures Position-Risk of Sub-account

## HTTP Request​

GET `/sapi/v1/sub-account/futures/positionRisk`

## Request Weight(IP)​

**10**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub-account email |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
[  
    {  
        "entryPrice": "9975.12000",  
        "leverage": "50",              // current initial leverage  
        "maxNotional": "1000000",      // notional value limit of current initial leverage  
        "liquidationPrice": "7963.54",  
        "markPrice": "9973.50770517",  
        "positionAmount": "0.010",  
        "symbol": "BTCUSDT",  
        "unrealizedProfit": "-0.01612295"  
    }  
]
```

