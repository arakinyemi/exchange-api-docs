# Flexible Loan Repay

## API Description​

Flexible Loan Repay

## HTTP Request​

POST `/sapi/v2/loan/flexible/repay`

## Request Weight​

**6000**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| loanCoin | STRING | YES |  |
| collateralCoin | STRING | YES |  |
| repayAmount | DECIMAL | YES |  |
| collateralReturn | BOOLEAN | NO | Default: TRUE. TRUE: Return extra collateral to spot account; FALSE: Keep extra collateral in the order, and lower LTV. |
| fullRepayment | BOOLEAN | NO | Default: FALSE. TRUE: Full repayment; FALSE: Partial repayment, based on loanAmount |
| repaymentType | INT | NO | Default: 1. 1: Repayment with loan asset; 2: Repayment with collateral |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * repayAmount is mandatory even fullRepayment = FALSE

## Response Example​

```json
{  
  "loanCoin": "BUSD",  
  "collateralCoin": "BNB",  
  "remainingDebt": "100.5",  
  "remainingCollateral": "5.253",  
  "fullRepayment": false,  
  "currentLTV": "0.25",  
  "repayStatus": "REPAID" // REPAID, REPAYING, FAILED  
}
```
