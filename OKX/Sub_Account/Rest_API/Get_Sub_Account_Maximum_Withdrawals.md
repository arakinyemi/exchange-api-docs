### Get sub-account maximum withdrawals

Retrieve the maximum withdrawal information of a sub-account via the master account (applies to master accounts only). If no currency is specified, the transferable amount of all owned currencies will be returned.

**Rate limit**: 20 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/account/subaccount/max-withdrawal`

Request Example
```
GET /api/v5/account/subaccount/max-withdrawal?subAcct=test1
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| ccy | String | No | Single currency or multiple currencies (no more than 20) separated with comma |

Response Example
```
{
   "code":"0",
   "data":[
      {
         "ccy":"BTC",
         "maxWd":"3",
         "maxWdEx":"",
         "spotOffsetMaxWd":"3",
         "spotOffsetMaxWdEx":""
      },
      {
         "ccy":"ETH",
         "maxWd":"15",
         "maxWdEx":"",
         "spotOffsetMaxWd":"15",
         "spotOffsetMaxWdEx":""
      },
      {
         "ccy":"USDT",
         "maxWd":"10600",
         "maxWdEx":"",
         "spotOffsetMaxWd":"10600",
         "spotOffsetMaxWdEx":""
      }
   ],
   "msg":""
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency |
| maxWd | String | Max withdrawal (excluding borrowed assets under Multi-currency margin) |
| maxWdEx | String | Max withdrawal (including borrowed assets under Multi-currency margin) |
| spotOffsetMaxWd | String | Max withdrawal under Spot-Derivatives risk offset mode (excluding borrowed assets under Portfolio margin) |
| spotOffsetMaxWdEx | String | Max withdrawal under Spot-Derivatives risk offset mode (including borrowed assets under Portfolio margin) |
