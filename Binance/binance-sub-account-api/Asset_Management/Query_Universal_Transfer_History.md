# Query Universal Transfer History (For Master Account) 

## API Description​

Query Universal Transfer History

## HTTP Request​

GET `/sapi/v1/sub-account/universalTransfer`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| fromEmail | STRING | NO |  |
| toEmail | STRING | NO |  |
| clientTranId | STRING | NO |  |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| page | INT | NO | Default 1 |
| limit | INT | NO | Default 500, Max 500 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * fromEmail and toEmail cannot be sent at the same time.
> * Return fromEmail equal master account email by default.
> * The query time period must be less than 7 days.
> * If startTime and endTime not sent, return records of the last 7 days by default.

## Response Example​

```
{  
    "result": [  
        {  
            "tranId": 92275823339,  
            "fromEmail": "abctest@gmail.com",  
            "toEmail": "deftest@gmail.com",  
            "asset": "BNB",  
            "amount": "0.01",  
            "createTimeStamp": 1640317374000,  
            "fromAccountType": "USDT_FUTURE",  
            "toAccountType": "SPOT",  
            "status": "SUCCESS",  
            "clientTranId": "test"  
        }  
    ],  
    "totalCount": 1  
}
```

