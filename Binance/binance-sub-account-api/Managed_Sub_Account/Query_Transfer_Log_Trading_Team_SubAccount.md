# Query Managed Sub Account Transfer Log (For Trading Team Sub Account) 

## API Description​

Query Managed Sub Account Transfer Log (For Trading Team Sub Account)

## HTTP Request​

GET `/sapi/v1/managed-subaccount/query-trans-log`

## Request Weight(UID)​

**60**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| startTime | LONG | YES | Start Time |
| endTime | LONG | YES | End Time (The start time and end time interval cannot exceed half a year) |
| page | INT | YES | Page |
| limit | INT | YES | Limit (Max: 500) |
| transfers | STRING | NO | Transfer Direction (FROM/TO) |
| transferFunctionAccountType | STRING | NO | Transfer function account type (SPOT/MARGIN/ISOLATED\_MARGIN/USDT\_FUTURE/COIN\_FUTURE) |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
    "managerSubTransferHistoryVos": [  
        {  
            "fromEmail": "test_0_virtual@kq3kno9imanagedsub.com",  
            "fromAccountType": "SPOT",  
            "toEmail": "wdywl0lddakh@test.com",  
            "toAccountType": "SPOT",  
            "asset": "BNB",  
            "amount": "0.01",  
            "scheduledData": 1679416673000,  
            "createTime": 1679416673000,  
            "status": "SUCCESS",  
            "tranId": 91077779  
        },  
        {  
            "fromEmail": "wdywl0lddakh@test.com",  
            "fromAccountType": "SPOT",  
            "toEmail": "test_0_virtual@kq3kno9imanagedsub.com",  
            "toAccountType": "SPOT",  
            "asset": "BNB",  
            "amount": "1",  
            "scheduledData": 1679416616000,  
            "createTime": 1679416616000,  
            "status": "SUCCESS",  
            "tranId": 91077676  
        }  
    ],  
    "count": 2  
}
```

