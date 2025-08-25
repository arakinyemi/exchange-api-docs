# Get Loan Borrow History

## API Description​

Get Loan Borrow History

## HTTP Request​

GET `/sapi/v1/loan/borrow/history`

## Request Weight(IP)​

**400**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| orderId | LONG | NO | orderId in `POST /sapi/v1/loan/borrow` |
| loanCoin | STRING | NO |  |
| collateralCoin | STRING | NO |  |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| current | LONG | NO | Current querying page. Start from 1; default: 1; max: 1000. |
| limit | LONG | NO | Default: 10; max: 100. |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If startTime and endTime are not sent, the recent 90-day data will be returned.
> * The max interval between startTime and endTime is 180 days.

## Response Example​

```json
{  
  "rows": [  
    {  
    "orderId": 100000001,  
    "loanCoin": "BUSD",  
    "initialLoanAmount": "10000",  
    "hourlyInterestRate": "0.000057"  
    "loanTerm": "7"  
    "collateralCoin": "BNB",  
    "initialCollateralAmount": "49.27565492"  
    "borrowTime": 1575018510000  
    "status": "Repaid" // Accruing_Interest, Overdue, Liquidating, Repaying, Repaid, Liquidated, Pending, Failed  
    }  
  ],  
  "total": 1  
}
```

