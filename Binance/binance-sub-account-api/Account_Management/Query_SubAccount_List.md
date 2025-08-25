# Query Sub-account List (For Master Account) 

## API Description​

Query Sub-account List

## HTTP Request​

GET `/sapi/v1/sub-account/list`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | NO | Sub-account email |
| isFreeze | STRING | NO | true or false |
| page | INT | NO | Default value: 1 |
| limit | INT | NO | Default value: 1, Max value: 200 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
{  
    "subAccounts":[  
        {  
            "subUserId": 123456,  
            "email":"testsub@gmail.com",  
            "remark": "remark",  
            "isFreeze":false,  
            "createTime":1544433328000,  
            "isManagedSubAccount": false,  
            "isAssetManagementSubAccount": false  
        },  
        {  
            "subUserId": 1234567,  
            "email":"virtual@oxebmvfonoemail.com",  
            "remark": "remarks",  
            "isFreeze":false,  
            "createTime":1544433328000,  
            "isManagedSubAccount": false,  
            "isAssetManagementSubAccount": false  
        }  
    ]  
}
```

