# Query Managed Sub Account Transfer Log (For Investor Master Account) 

## API Description​

Investor can use this api to query managed sub account transfer log. This endpoint is available for investor of Managed Sub-Account. A Managed Sub-Account is an account type for investors who value flexibility in asset allocation and account application, while delegating trades to a professional trading team.
Please refer to link

## HTTP Request​

GET `/sapi/v1/managed-subaccount/queryTransLogForInvestor`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Managed Sub Account Email |
| startTime | LONG | YES | Start Time |
| endTime | LONG | YES | End Time (The start time and end time interval cannot exceed half a year) |
| page | INT | YES | Page |
| limit | INT | YES | Limit (Max: 500) |
| transfers | STRING | NO | Transfer Direction (FROM/TO) |
| transferFunctionAccountType | STRING | NO | Transfer function account type (SPOT/MARGIN/ISOLATED\_MARGIN/USDT\_FUTURE/COIN\_FUTURE) |

## Response Example​

```
{  
    "managerSubTransferHistoryVos": [  
        {  
            "fromEmail": "test_0_virtual@kq3kno9imanagedsub.com"  
            "fromAccountType": "SPOT"  
            "toEmail": "wdywl0lddakh@test.com"  
            "toAccountType": "SPOT"  
            "asset": "BNB"  
            "amount": "0.01"  
            "scheduledData": 1679416673000  
            "createTime": 1679416673000  
            "status": "SUCCESS"  
            "tranId": 91077779  
        },  
        {  
            "fromEmail": "wdywl0lddakh@test.com"  
            "fromAccountType": "SPOT"  
            "toEmail": "test_0_virtual@kq3kno9imanagedsub.com"  
            "toAccountType": "SPOT"  
            "asset": "BNB"  
            "amount": "1"  
            "scheduledData": 1679416616000  
            "createTime": 1679416616000  
            "status": "SUCCESS"  
            "tranId": 91077676  
        }  
    ],  
    "count": 2  
}
```

