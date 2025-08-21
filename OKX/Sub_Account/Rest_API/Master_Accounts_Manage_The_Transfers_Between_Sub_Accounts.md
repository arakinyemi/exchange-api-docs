### Master accounts manage the transfers between sub-accounts

Applies to master accounts only. Only API keys with Trade privilege can call this endpoint.

**Rate limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

HTTP Request
`POST /api/v5/asset/subaccount/transfer`

Request Example
```
POST /api/v5/asset/subaccount/transfer
body
{
    "ccy":"USDT",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "fromSubAccount":"test-1",
    "toSubAccount":"test-2"
}
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | Yes | Currency |
| amt | String | Yes | Transfer amount |
| from | String | Yes | Account type of transfer from sub-account (6: Funding, 18: Trading) |
| to | String | Yes | Account type of transfer to sub-account (6: Funding, 18: Trading) |
| fromSubAccount | String | Yes | Sub-account name transferring out |
| toSubAccount | String | Yes | Sub-account name transferring in |
| loanTrans | Boolean | No | Whether borrowed coins can be transferred out (default false) |
| omitPosRisk | String | No | Ignore position risk (default false) |

Response Example
```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "transId":"12345"
        }
    ]
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| transId | String | Transfer ID |
