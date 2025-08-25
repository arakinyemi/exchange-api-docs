# Get API Key Permission 

## API Description​

Get API Key Permission

## HTTP Request​

GET `/sapi/v1/account/apiRestrictions`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
{  
  "ipRestrict":false,   
  "createTime":1698645219000,  
  "enableReading":true,   
  "enableWithdrawals":false, // This option allows you to withdraw via API. You must apply the IP Access Restriction filter in order to enable withdrawals  
  "enableInternalTransfer":false,  // This option authorizes this key to transfer funds between your master account and your sub account instantly  
  "enableMargin":false,  //  This option can be adjusted after the Cross Margin account transfer is completed  
  "enableFutures":false,   //  The Futures API cannot be used if the API key was created before the Futures account was opened, or if you have enabled portfolio margin.  
  "permitsUniversalTransfer":false, // Authorizes this key to be used for a dedicated universal transfer API to transfer multiple supported currencies. Each business's own transfer API rights are not affected by this authorization  
  "enableVanillaOptions":false,  //  Authorizes this key to Vanilla options trading  
  "enableFixApiTrade": false,   //  
  "enableFixReadOnly": true,  
  "enableSpotAndMarginTrading":false, // Spot and margin trading  
  "enablePortfolioMarginTrading":true  //  API Key created before your activate portfolio margin does not support portfolio margin API service  
}
```

