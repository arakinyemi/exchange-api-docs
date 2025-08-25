# Flexible Loan Borrow

## API Description​

Borrow Flexible Loan

## HTTP Request​

POST `/sapi/v2/loan/flexible/borrow`

## Request Weight​

**6000**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| loanCoin | STRING | YES |  |
| loanAmount | DECIMAL | NO | Mandatory when collateralAmount is empty |
| collateralCoin | STRING | YES |  |
| collateralAmount | DECIMAL | NO | Mandatory when loanAmount is empty |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |



* Only available for master account
* You can customize LTV by entering loanAmount and collateralAmount.

## Response Example​

```json
{  
  "loanCoin": "BUSD",  
  "loanAmount": "100.5",  
  "collateralCoin": "BNB",  
  "collateralAmount": "50.5",  
  "status": "Succeeds" //Succeeds, Failed, Processing  
}
```
