# Get Summary of Sub-account's Futures Account V2 (For Master Account) 

## API Description​

Get Summary of Sub-account's Futures Account

## HTTP Request​

GET `/sapi/v2/sub-account/futures/accountSummary`

## Request Weight(IP)​

**10**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| futuresType | INT | YES | 1:USDT Margined Futures, 2:COIN Margined Futures |
| page | INT | NO | default:1 |
| limit | INT | NO | default:10, max:20 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

> USDT Margined Futures：

```
{  
  "futureAccountSummaryResp": {  
    "totalInitialMargin": "9.83137400",   
    "totalMaintenanceMargin": "0.41568700",   
    "totalMarginBalance": "23.03235621",   
    "totalOpenOrderInitialMargin": "9.00000000",  
    "totalPositionInitialMargin": "0.83137400",  
    "totalUnrealizedProfit": "0.03219710",  
    "totalWalletBalance": "22.15879444",  
    "asset": "USD",  //The sum of BUSD and USDT  
    "subAccountList":[  
        {  
            "email": "123@test.com",  
            "totalInitialMargin": "9.00000000",   
            "totalMaintenanceMargin": "0.00000000",   
            "totalMarginBalance": "22.12659734",   
            "totalOpenOrderInitialMargin": "9.00000000",  
            "totalPositionInitialMargin": "0.00000000",  
            "totalUnrealizedProfit": "0.00000000",  
            "totalWalletBalance": "22.12659734",  
            "asset": "USD"  //The sum of BUSD and USDT  
        },  
        {   
            "email": "345@test.com",  
            "totalInitialMargin": "0.83137400",   
            "totalMaintenanceMargin": "0.41568700",  
            "totalMarginBalance": "0.90575887",  
            "totalOpenOrderInitialMargin": "0.00000000",  
            "totalPositionInitialMargin": "0.83137400",  
            "totalUnrealizedProfit": "0.03219710",  
            "totalWalletBalance": "0.87356177",  
            "asset": "USD"  
        }  
    ]  
  }  
}
```

> COIN Margined Futures：

```
{  
  "deliveryAccountSummaryResp": {  
    "totalMarginBalanceOfBTC": "25.03221121",   
    "totalUnrealizedProfitOfBTC": "0.12233410",  
    "totalWalletBalanceOfBTC": "22.15879444",  
    "asset": "BTC",  
    "subAccountList":[  
        {  
            "email": "123@test.com",  
            "totalMarginBalance": "22.12659734",   
            "totalUnrealizedProfit": "0.00000000",  
            "totalWalletBalance": "22.12659734",  
            "asset": "BTC"  
        },  
        {   
            "email": "345@test.com",  
            "totalMarginBalance": "0.90575887",  
            "totalUnrealizedProfit": "0.03219710",  
            "totalWalletBalance": "0.87356177",  
            "asset": "BTC"  
        }  
    ]  
  }  
}
```

