# Query Managed Sub-account Snapshot (For Investor Master Account) 

## API Description​

Query Managed Sub-account Snapshot

## HTTP Request​

GET `/sapi/v1/managed-subaccount/accountSnapshot`

## Request Weight(IP)​

**2400**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES |  |
| type | STRING | YES | "SPOT", "MARGIN"（cross）, "FUTURES"（UM） |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| limit | INT | NO | min 7, max 30, default 7 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * The query time period must be less then 30 days
> * Support query within the last one month only
> * If startTimeand endTime not sent, return records of the last 7 days by default

## Response Example​

```
{  
   "code":200, // 200 for success; others are error codes  
   "msg":"", // error message  
   "snapshotVos":[  
      {  
         "data":{  
            "balances":[  
               {  
                  "asset":"BTC",  
                  "free":"0.09905021",  
                  "locked":"0.00000000"  
               },  
               {  
                  "asset":"USDT",  
                  "free":"1.89109409",  
                  "locked":"0.00000000"  
               }  
            ],  
            "totalAssetOfBtc":"0.09942700"  
         },  
         "type":"spot",  
         "updateTime":1576281599000  
      }  
   ]  
}
```

> OR

```
{  
   "code":200, // 200 for success; others are error codes  
   "msg":"", // error message  
   "snapshotVos":[  
      {  
         "data":{  
            "marginLevel":"2748.02909813",  
            "totalAssetOfBtc":"0.00274803",  
            "totalLiabilityOfBtc":"0.00000100",  
            "totalNetAssetOfBtc":"0.00274750",  
            "userAssets":[  
               {  
                  "asset":"XRP",  
                  "borrowed":"0.00000000",  
                  "free":"1.00000000",  
                  "interest":"0.00000000",  
                  "locked":"0.00000000",  
                  "netAsset":"1.00000000"  
               }  
            ]  
         },  
         "type":"margin",  
         "updateTime":1576281599000  
      }  
   ]  
}
```

> OR

```
{  
   "code":200, // 200 for success; others are error codes  
   "msg":"", // error message  
   "snapshotVos":[  
      {  
         "data":{  
            "assets":[  
               {  
                  "asset":"USDT",  
                  "marginBalance":"118.99782335",  
                  "walletBalance":"120.23811389"  
               }  
            ],  
            "position":[  
               {  
                  "entryPrice":"7130.41000000",  
                  "markPrice":"7257.66239673",  
                  "positionAmt":"0.01000000",  
                  "symbol":"BTCUSDT",  
                  "unRealizedProfit":"1.24029054"  // Only show the value at the time of opening the position  
               }  
            ]  
         },  
         "type":"futures",  
         "updateTime":1576281599000  
      }  
   ]  
}
```

