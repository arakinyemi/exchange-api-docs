# Get Flexible Loan Ongoing Orders

## API Description​

Get Flexible Loan Ongoing Orders

## HTTP Request​

GET `/sapi/v2/loan/flexible/ongoing/orders`

## Request Weight(IP)​

**300**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| loanCoin | STRING | NO |  |
| collateralCoin | STRING | NO |  |
| current | LONG | NO | Current querying page. Start from 1; default: 1; max: 1000 |
| limit | LONG | NO | Default: 10; max: 100 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
  "rows": [  
    {  
      "loanCoin": "BUSD",  
      "totalDebt": "10000",  
      "collateralCoin": "BNB",  
      "collateralAmount": "49.27565492",  
      "currentLTV": "0.57"  
    }  
  ],  
  "total": 1  
}
```

