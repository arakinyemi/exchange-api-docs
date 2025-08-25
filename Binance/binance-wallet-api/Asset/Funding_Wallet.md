# Funding Wallet 

## API Description​

Query Funding Wallet

## HTTP Request​

POST `/sapi/v1/asset/get-funding-asset`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| asset | STRING | NO |  |
| needBtcValuation | STRING | NO | true or false |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * Currently supports querying the following business assets：Binance Pay, Binance Card, Binance Gift Card, Stock Token

## Response Example​

```json
[  
    {  
        "asset": "USDT",  
        "free": "1",    // avalible balance  
        "locked": "0",  // locked asset  
        "freeze": "0",  // freeze asset  
        "withdrawing": "0",    
        "btcValuation": "0.00000091"    
    }  
]
```

