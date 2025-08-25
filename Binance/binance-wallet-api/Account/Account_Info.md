# Account Info 

## API Description​

Fetch account info detail.

## HTTP Request​

GET `/sapi/v1/account/info`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
  "vipLevel": 0,  
  "isMarginEnabled": true,     // true or false for margin.  
  "isFutureEnabled": true,      // true or false for futures.  
  "isOptionsEnabled":true,      // true or false for options.  
  "isPortfolioMarginRetailEnabled":true      // true or false for portfolio margin retail.  
}
```

