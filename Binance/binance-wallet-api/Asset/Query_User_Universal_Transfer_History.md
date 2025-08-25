# Query User Universal Transfer History

## API Description​

Query User Universal Transfer History

## HTTP Request​

GET `/sapi/v1/asset/transfer`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| type | ENUM | YES |  |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| current | INT | NO | Default 1 |
| size | INT | NO | Default 10, Max 100 |
| fromSymbol | STRING | NO |  |
| toSymbol | STRING | NO |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * `fromSymbol` must be sent when type are ISOLATEDMARGIN\_MARGIN and ISOLATEDMARGIN\_ISOLATEDMARGIN
> * `toSymbol` must be sent when type are MARGIN\_ISOLATEDMARGIN and ISOLATEDMARGIN\_ISOLATEDMARGIN
> * Support query within the last 6 months only
> * If `startTime`and `endTime` not sent, return records of the last 7 days by default

## Response Example​

```json
{  
    "total":2,  
    "rows":[  
        {  
            "asset":"USDT",  
            "amount":"1",  
            "type":"MAIN_UMFUTURE",  
            "status": "CONFIRMED", // status: CONFIRMED / FAILED / PENDING  
            "tranId": 11415955596,  
            "timestamp":1544433328000  
        },  
        {  
            "asset":"USDT",  
            "amount":"2",  
            "type":"MAIN_UMFUTURE",  
            "status": "CONFIRMED",  
            "tranId": 11366865406,  
            "timestamp":1544433328000  
        }  
    ]  
}
```

