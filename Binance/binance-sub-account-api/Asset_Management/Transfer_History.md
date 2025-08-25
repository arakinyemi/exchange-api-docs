# Sub-account Transfer History (For Sub-account) 

## API Description​

Sub-account Transfer History

## HTTP Request​

GET `/sapi/v1/sub-account/transfer/subUserHistory`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| asset | STRING | NO | If not sent, result of all assets will be returned |
| type | INT | NO | 1: transfer in, 2: transfer out |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| limit | INT | NO | Default 500 |
| returnFailHistory | BOOLEAN | NO | Default `False`, return PROCESS and SUCCESS status history; If `True`,return PROCESS and SUCCESS and FAILURE status history |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If type is not sent, the records of type 2: transfer out will be returned by default.
> * If startTime and endTime are not sent, the recent 30-day data will be returned.

## Response Example​

```
[  
  {  
    "counterParty":"master",  
    "email":"master@test.com",  
    "type":1,  // 1 for transfer in, 2 for transfer out  
    "asset":"BTC",  
    "qty":"1",  
    "fromAccountType":"SPOT",  
    "toAccountType":"SPOT",  
    "status":"SUCCESS", // status: PROCESS / SUCCESS / FAILURE  
    "tranId":11798835829,  
    "time":1544433325000  
  },  
  {  
    "counterParty":"subAccount",  
    "email":"sub2@test.com",  
    "type":1,                                   
    "asset":"ETH",  
    "qty":"2",  
    "fromAccountType":"SPOT",  
    "toAccountType":"COIN_FUTURE",  
    "status":"SUCCESS",  
    "tranId":11798829519,  
    "time":1544433326000  
  }  
]
```

