# Flexible Loan Adjust LTV

## API Description​

Flexible Loan Adjust LTV

## HTTP Request​

POST `/sapi/v2/loan/flexible/adjust/ltv`

## Request Weight(UID)​

**6000**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| loanCoin | STRING | YES |  |
| collateralCoin | STRING | YES |  |
| adjustmentAmount | DECIMAL | YES |  |
| direction | ENUM | YES | "ADDITIONAL", "REDUCED" |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * API Key needs Spot & Margin Trading permission for this endpoint

## Response Example​

```json
{  
  "loanCoin": "BUSD",  
  "collateralCoin": "BNB",  
  "direction": "ADDITIONAL",  
  "adjustmentAmount": "5.235",  
  "currentLTV": "0.52",  
  "status": "Succeeds" // "Succeeds", "Failed", "Processing"  
}
```
