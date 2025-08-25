# Get Futures Position-Risk of Sub-account V2 (For Master Account) 

## API Description​

Get Futures Position-Risk of Sub-account V2

## HTTP Request​

GET `/sapi/v2/sub-account/futures/positionRisk`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub-account email |
| futuresType | INT | YES | 1:USDT Margined Futures, 2:COIN Margined Futures |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

> USDT Margined Futures：

```
{  
  "futurePositionRiskVos": [  
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
}
```

> COIN Margined Futures：

```
{  
  "deliveryPositionRiskVos": [  
     {  
        "entryPrice": "9975.12000",  
        "markPrice": "9973.50770517",  
        "leverage": "20",            
        "isolated": "false",                  
        "isolatedWallet": "9973.50770517",  
        "isolatedMargin": "0.00000000",  
        "isAutoAddMargin": "false",  
        "positionSide": "BOTH",  
        "positionAmount": "1.230",  
        "symbol": "BTCUSD_201225",  
        "unrealizedProfit": "-0.01612295"  
     }  
   ]  
}
```

