# Get Flexible Loan Collateral Assets Data

## API Description​

Get LTV information and collateral limit of flexible loan's collateral assets. The collateral limit is shown in USD value.

## HTTP Request​

GET `/sapi/v2/loan/flexible/collateral/data`

## Request Weight(IP)​

**400**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| collateralCoin | STRING | NO |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
  "rows": [  
    {  
      "collateralCoin": "BNB",  
      "initialLTV": "0.65",  
      "marginCallLTV": "0.75",  
      "liquidationLTV": "0.83",  
      "maxLimit": "1000000"  
    }  
  ],  
  "total": 1  
}
```

