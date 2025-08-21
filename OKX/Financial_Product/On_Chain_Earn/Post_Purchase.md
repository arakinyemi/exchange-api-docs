### POST / Purchase
**Rate Limit**: 2 requests per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

HTTP Request
`POST /api/v5/finance/staking-defi/purchase`

Request Example
```
POST /api/v5/finance/staking-defi/purchase
body 
{
    "productId":"1234",
    "investData":[
      {
        "ccy":"ZIL",
        "amt":"100"
      }
    ],
    "term":"30"
}
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| productId | String | Yes | Product ID |
| investData | Array | Yes | Investment data |
| term | String | Conditional | Investment term |
| tag | String | No | Order tag |

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
