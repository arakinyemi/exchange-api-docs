# Get Detail on Sub-account's Futures Account V2 (For Master Account) 

## API Description​

Get Detail on Sub-account's Futures Account

## HTTP Request​

GET `/sapi/v2/sub-account/futures/account`

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
	"futureAccountResp": {  
	"email": "abc@test.com",  
	"assets":[  
		{  
		  	"asset": "USDT",  
		   	"initialMargin": "0.00000000",  
		   	"maintenanceMargin": "0.00000000",  
		   	"marginBalance": "0.88308000",  
		   	"maxWithdrawAmount": "0.88308000",  
		   	"openOrderInitialMargin": "0.00000000",  
		   	"positionInitialMargin": "0.00000000",  
		   	"unrealizedProfit": "0.00000000",  
		   	"walletBalance": "0.88308000"  
		 }  
	],  
	"canDeposit": true,  
	"canTrade": true,  
	"canWithdraw": true,  
	"feeTier": 2,  
	"maxWithdrawAmount": "0.88308000",  
	"totalInitialMargin": "0.00000000",  
	"totalMaintenanceMargin": "0.00000000",  
	"totalMarginBalance": "0.88308000",  
	"totalOpenOrderInitialMargin": "0.00000000",  
	"totalPositionInitialMargin": "0.00000000",  
	"totalUnrealizedProfit": "0.00000000",  
	"totalWalletBalance": "0.88308000",  
	"updateTime": 1576756674610  
 }  
}
```

> COIN Margined Futures：

```
{  
	"deliveryAccountResp": {  
        "email": "abc@test.com",  
        "assets":[  
            {  
                "asset": "BTC",  
                "initialMargin": "0.00000000",  
                "maintenanceMargin": "0.00000000",  
                "marginBalance": "0.88308000",  
                "maxWithdrawAmount": "0.88308000",  
                "openOrderInitialMargin": "0.00000000",  
                "positionInitialMargin": "0.00000000",  
                "unrealizedProfit": "0.00000000",  
                "walletBalance": "0.88308000"  
             }  
        ],  
        "canDeposit": true,  
        "canTrade": true,  
        "canWithdraw": true,  
        "feeTier": 2,  
        "updateTime": 1598959682001  
    }  
}
```

