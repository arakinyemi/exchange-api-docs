# Query Managed Sub-account List (For Investor) 

## API Description​

Get investor's managed sub-account list.

## HTTP Request​

GET `/sapi/v1/managed-subaccount/info`

## Request Weight(UID)​

**60**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | NO | Managed sub-account email |
| page | INT | NO | Default value: 1 |
| limit | INT | NO | Default value: 20, Max value: 20 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
    "total": 3,  
    "managerSubUserInfoVoList": [  
        {  
            "rootUserId": 1000138475670  
            "managersubUserId": 1000137842513  
            "bindParentUserId": 1000138475669  
            "email": "test_0_virtual@kq3kno9imanagedsub.com"  
            "insertTimeStamp": 1678435149000  
            "bindParentEmail": "wdyw8xsh8pey@test.com"  
            "isSubUserEnabled": true  
            "isUserActive": true  
            "isMarginEnabled": false  
            "isFutureEnabled": false  
            "isSignedLVTRiskAgreement": false  
        },  
        {  
            "rootUserId": 1000138475670  
            "managersubUserId": 1000137842514  
            "bindParentUserId": 1000138475669  
            "email": "test_1_virtual@4qd2u7zxmanagedsub.com"  
            "insertTimeStamp": 1678435152000  
            "bindParentEmail": "wdyw8xsh8pey@test.com"  
            "isSubUserEnabled": true  
            "isUserActive": true  
            "isMarginEnabled": false  
            "isFutureEnabled": false  
            "isSignedLVTRiskAgreement": false  
        },  
        {  
            "rootUserId": 1000138475670  
            "managersubUserId": 1000137842515  
            "bindParentUserId": 1000138475669  
            "email": "test_2_virtual@akc05o8hmanagedsub.com"  
            "insertTimeStamp": 1678435153000  
            "bindParentEmail": "wdyw8xsh8pey@test.com"  
            "isSubUserEnabled": true  
            "isUserActive": true  
            "isMarginEnabled": false  
            "isFutureEnabled": false  
            "isSignedLVTRiskAgreement": false  
        }  
    ]  
}
```

