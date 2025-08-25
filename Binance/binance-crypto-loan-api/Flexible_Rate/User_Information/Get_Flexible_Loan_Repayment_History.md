# Get Flexible Loan Repayment History

## API Description​

Get Flexible Loan Repayment History

## HTTP Request​

GET `/sapi/v2/loan/flexible/repay/history`

GET `/sapi/v1/loan/flexible/repay/history` can be used to check history before 2024-02-27 08:00

## Request Weight(IP)​

**400**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| loanCoin | STRING | NO |  |
| collateralCoin | STRING | NO |  |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| current | LONG | NO | Current querying page. Start from 1; default: 1; max: 1000 |
| limit | LONG | NO | Default: 10; max: 100 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If startTime and endTime are not sent, the recent 90-day data will be returned.
> * The max interval between startTime and endTime is 180 days.

## Response Example​

```
{  
  "rows": [  
    {  
      "loanCoin": "BUSD",  
      "repayAmount": "10000",  
      "collateralCoin": "BNB",  
      "collateralReturn": "49.27565492",  
      "repayStatus": "REPAID" // REPAID, REPAYING, FAILED  
      "repayTime": 1575018510000  
    }  
  ],  
  "total": 1  
}
```

