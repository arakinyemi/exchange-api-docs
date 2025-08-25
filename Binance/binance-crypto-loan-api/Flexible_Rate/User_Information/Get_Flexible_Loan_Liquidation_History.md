# Get Flexible Loan Liquidation History 

## HTTP Request​

`GET /sapi/v2/loan/flexible/liquidation/history`

## Request Weight (IP)​

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

* If startTime and endTime are not sent, the recent 90-day data will be returned.
* The max interval between startTime and endTime is 180 days.

## Response Example​

```
{  
  "rows": [  
    {  
      "loanCoin": "BUSD",  
      "liquidationDebt": "10000",  
      "collateralCoin": "BNB",  
      "liquidationCollateralAmount": "123",  
      "returnCollateralAmount": "0.2",  
      "liquidationFee": "1.2",  
      "liquidationStartingPrice": "49.27565492",  
      "liquidationStartingTime": 1575018510000,  
      "status": "Liquidated" // Liquidating  
    }  
  ],  
  "total": 1  
}
```

liquidationStartingPrice refers to the Collateral/borrowed token exchange rate at the start of the liquidation time. The final exchange rate will vary depending on token liquidity and market conditions.

* HTTP Request
* Request Weight (IP)
* Request Parameters
* Response Example