# Get Loan LTV Adjustment History

## API Description​

Get Loan LTV Adjustment History

## HTTP Request​

GET `/sapi/v1/loan/ltv/adjustment/history`

## Request Weight​

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
    "collateralCoin": "BNB",  
    "direction": "ADDITIONAL",  
    "amount": "5.235",  
    "preLTV": "0.78",  
    "afterLTV": "0.56",  
    "adjustTime": 1575018510000,  
    "orderId": 756783308056935434  
    }  
  ],  
  "total": 1  
}
```
