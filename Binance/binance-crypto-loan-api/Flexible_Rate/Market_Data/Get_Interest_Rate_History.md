# Get Flexible Loan Interest Rate History 

**API Description**
Check Flexible Loan interest rate history

**HTTP Request**

GET/sapi/v2/loan/interestRateHistory

**Request Weight(IP):**

400

**Request Parameter:**

| **Name** | **Type** | **Mandatory** | **Description** |
| --- | --- | --- | --- |
| coin | STRING | YES |  |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| current | LONG | NO | Check current querying page, start from 1. Default：1；Max：1000. |
| limit | LONG | NO | Default：10; Max：100. |
| recvWindow | LONG | YES |  |
| timestamp | LONG | YES |  |

* If startTime and endTime are not sent, the recent 90-day data will be returned
* The max interval between startTime and endTime is 90 days.
* Time based on UTC+0.

**Response Example：**

```json
{  
    "rows": [  
        {  
            "coin": "USDT",  
            "annualizedInterestRate": "0.0647",  
            "time": 1575018510000  
        },  
        {  
            "coin": "USDT",  
            "annualizedInterestRate": "0.0647",  
            "time": 1575018510000  
        }  
    ],  
    "total": 2  
}
```