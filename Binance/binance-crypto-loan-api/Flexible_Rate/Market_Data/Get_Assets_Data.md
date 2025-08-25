# Get Flexible Loan Assets Data

## API Description​

Get interest rate and borrow limit of flexible loanable assets. The borrow limit is shown in USD value.

## HTTP Request​

GET `/sapi/v2/loan/flexible/loanable/data`

## Request Weight(IP)​

**400**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| loanCoin | STRING | NO |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
{  
  "rows": [  
    {  
      "loanCoin": "BUSD",  
      "flexibleInterestRate": "0.00000491",  
      "flexibleMinLimit": "100",  
      "flexibleMaxLimit": "1000000"  
    }  
  ],  
  "total": 1  
}
```

