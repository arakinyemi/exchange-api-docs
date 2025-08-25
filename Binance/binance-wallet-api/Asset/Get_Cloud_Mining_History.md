# Get Cloud-Mining payment and refund history 

## API Description​

The query of Cloud-Mining payment and refund history

## HTTP Request​

GET `/sapi/v1/asset/ledger-transfer/cloud-mining/queryByPage`

## Request Weight(UID)​

**600**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| tranId | LONG | NO | The transaction id |
| clientTranId | STRING | NO | The unique flag |
| asset | STRING | NO | If it is blank, we will query all assets |
| startTime | LONG | YES | inclusive, unit: ms |
| endTime | LONG | YES | exclusive, unit: ms |
| current | INTEGER | NO | current page, default 1, the min value is 1 |
| size | INTEGER | NO | page size, default 10, the max value is 100 |

> * Just return the SUCCESS records of payment and refund.
> * For response, type = 248 means payment, type = 249 means refund, status =S means SUCCESS.

## Response Example​

```json
{  
  "total":5,  
  "rows":[  
    {"createTime":1667880112000,"tranId":121230610120,"type":248,"asset":"USDT","amount":"25.0068","status":"S"},  
    {"createTime":1666776366000,"tranId":119991507468,"type":249,"asset":"USDT","amount":"0.027","status":"S"},  
    {"createTime":1666764505000,"tranId":119977966327,"type":248,"asset":"USDT","amount":"0.027","status":"S"},  
    {"createTime":1666758189000,"tranId":119973601721,"type":248,"asset":"USDT","amount":"0.018","status":"S"},  
    {"createTime":1666757278000,"tranId":119973028551,"type":248,"asset":"USDT","amount":"0.018","status":"S"}  
  ]  
}
```

