### POST / Redeem
**Rate Limit**: 2 requests per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

HTTP Request
`POST /api/v5/finance/staking-defi/redeem`

Request Example
```
POST /api/v5/finance/staking-defi/redeem
body 
{
    "ordId":"754147",
    "protocolType":"defi",
    "allowEarlyRedeem":true
}
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ordId | String | Yes | Order ID |
| protocolType | String | Yes | Protocol type |
| allowEarlyRedeem | Boolean | No | Early redemption flag |

Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "ordId": "754147",
      "tag": ""
    }
  ]
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ordId | String | Order ID |
| tag | String | Order tag |
