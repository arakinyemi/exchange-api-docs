# Get Loan Repayment History

## API Description​

Get Loan Repayment History

## HTTP Request​

GET `/sapi/v1/loan/repay/history`

## Request Weight(IP)​

**400**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| orderId | LONG | NO |  |
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

```json
{  
  "rows": [  
    {  
    "loanCoin": "BUSD",  
    "repayAmount": "10000",  
    "collateralCoin": "BNB",  
    "collateralUsed": "0"  ,
    "collateralReturn": "49.27565492"  ,
    "repayType": "1" ,// 1 for "repay with borrowed coin", 2 for "repay with collateral"  
    "repayStatus": "Repaid", // Repaid, Repaying, Failed  
    "repayTime": 1575018510000  ,
    "orderId": 756783308056935434  
    }  
  ],  
  "total": 1  
}
```

