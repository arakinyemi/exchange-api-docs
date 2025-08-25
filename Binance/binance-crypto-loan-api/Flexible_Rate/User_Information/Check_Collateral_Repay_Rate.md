# Check Collateral Repay Rate 

## HTTP Request​

`GET /sapi/v2/loan/flexible/repay/rate`

## Description​

Get the latest rate of collateral coin/loan coin when using collateral repay.

## Request Weight (IP)​

**6000**

## Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| loanCoin | STRING | YES |  |
| collateralCoin | STRING | YES |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
{  
  "loanCoin": "BUSD",  
  "collateralCoin": "BNB",  
  "rate": "300.36781234" // rate of collateral coin/loan coin  
}
```

* HTTP Request
* Description
* Request Weight (IP)
* Parameters
* Response Example