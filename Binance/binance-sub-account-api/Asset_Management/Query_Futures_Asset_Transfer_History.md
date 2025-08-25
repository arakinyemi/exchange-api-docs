# Query Sub-account Futures Asset Transfer History (For Master Account) 

## API Description​

Query Sub-account Futures Asset Transfer History

## HTTP Request​

GET `/sapi/v1/sub-account/futures/internalTransfer`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub-account email |
| futuresType | LONG | YES | 1:USDT-margined Futures，2: Coin-margined Futures |
| startTime | LONG | NO | Cannot be earlier than 1 month ago |
| endTime | LONG | NO |  |
| page | INT | NO | Default value: 1 |
| limit | INT | NO | Default value: 50, Max value: 500 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
    "success":true,  
    "futuresType": 2,  
    "transfers":[  
        {  
            "from":"aaa@test.com",  
            "to":"bbb@test.com",  
            "asset":"BTC",  
            "qty":"1",  
            "tranId":11897001102,  
            "time":1544433328000  
        },  
        {  
            "from":"bbb@test.com",  
            "to":"ccc@test.com",  
            "asset":"ETH",  
            "qty":"2",  
            "tranId":11631474902,  
            "time":1544433328000  
        }  
    ]  
}
```

