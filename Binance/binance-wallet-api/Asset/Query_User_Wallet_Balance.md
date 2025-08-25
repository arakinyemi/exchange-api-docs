# Query User Wallet Balance 

## API Description​

Query User Wallet Balance

## HTTP Request​

GET `/sapi/v1/asset/wallet/balance`

## Request Weight(IP)​

**60**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| quoteAsset | STRING | NO | `USDT`, `ETH`, `USDC`, `BNB`, etc. default `BTC` |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to open Permits Universal Transfer permission for the API Key which requests this endpoint.

## Response Example​

```json
[  
  {  
    "activate": true,  
    "balance": "0",  
    "walletName": "Spot"  
  },   
  {  
    "activate": true,  
    "balance": "0",  
    "walletName": "Funding"  
  },   
  {  
    "activate": true,  
    "balance": "0",  
    "walletName": "Cross Margin"  
  },   
  {  
    "activate": true,  
    "balance": "0",  
    "walletName": "Isolated Margin"  
  },   
  {  
    "activate": true,  
    "balance": "0.71842752",  
    "walletName": "USDⓈ-M Futures"  
  },   
  {  
    "activate": true,  
    "balance": "0",  
    "walletName": "COIN-M Futures"  
  },   
  {  
    "activate": true,  
    "balance": "0",  
    "walletName": "Earn"  
  },   
  {  
    "activate": false,  
    "balance": "0",  
    "walletName": "Options"  
  },  
  {  
      "activate": true,  
      "balance": "0",  
      "walletName": "Trading Bots"  
  },  
  {  
      "activate": true,  
      "balance": "0",  
      "walletName": "Copy Trading"  
  }  
]
```

