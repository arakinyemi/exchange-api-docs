# Query Sub-account Spot Asset Transfer History (For Master Account) 

## API Description​

Query Sub-account Spot Asset Transfer History

## HTTP Request​

GET `/sapi/v1/sub-account/sub/transfer/history`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| fromEmail | STRING | NO |  |
| toEmail | STRING | NO |  |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| page | INT | NO | Default value: 1 |
| limit | INT | NO | Default value: 500 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * fromEmail and toEmail cannot be sent at the same time.
> * Return fromEmail equal master account email by default.

## Response Example​

```
[  
    {  
        "from":"aaa@test.com",  
        "to":"bbb@test.com",  
        "asset":"BTC",  
        "qty":"10",  
        "status": "SUCCESS",  
        "tranId": 6489943656,  
        "time":1544433328000  
    },  
    {  
        "from":"bbb@test.com",  
        "to":"ccc@test.com",  
        "asset":"ETH",  
        "qty":"2",  
        "status": "SUCCESS",  
        "tranId": 6489938713,  
        "time":1544433328000  
    }  
]
```

