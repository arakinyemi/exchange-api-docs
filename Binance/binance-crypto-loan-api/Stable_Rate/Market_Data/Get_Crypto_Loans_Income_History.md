# Get Crypto Loans Income History

## API Description​

Get Crypto Loans Income History

## HTTP Request​

`GET /sapi/v1/loan/income`

## Request Weight(UID)​

**6000**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| asset | STRING | NO |  |
| type | STRING | NO | All types will be returned by default. Enum：`borrowIn` ,`collateralSpent`, `repayAmount`, `collateralReturn`(Collateral return after repayment), `addCollateral`, `removeCollateral`, `collateralReturnAfterLiquidation` |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| limit | INT | NO | default 20, max 100 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If startTime and endTime are not sent, the recent 7-day data will be returned.
> * The max interval between startTime and endTime is 30 days.

## Response Example​

```json
[  
  {  
    asset: "BUSD",  
    type: "borrowIn",  
    amount: "100",  
    timestamp: 1633771139847,  
    tranId: "80423589583",  
  },  
  {  
    asset: "BUSD",  
    type: "borrowIn",  
    amount: "100",  
    timestamp: 1634638371496,  
    tranId: "81685123491",  
  },  
]
```
