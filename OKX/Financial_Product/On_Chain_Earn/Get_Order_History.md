### GET / Order history
**Rate Limit**: 3 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/finance/staking-defi/orders-history`

Request Example
```
GET /api/v5/finance/staking-defi/orders-history
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| productId | String | No | Product ID |
| protocolType | String | No | Protocol type |
| ccy | String | No | Investment currency |
| after | String | No | Pagination (ordId) |
| before | String | No | Pagination (ordId) |
| limit | String | No | Number of results |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
       {
            "ordId": "1579252",
            "ccy": "DOT",
            "productId": "101",
            "state": "3",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1704",
            "investData": [
                {
                    "ccy": "DOT",
                    "amt": "2"
                }
            ],
            "earningData": [
                {
                    "ccy": "DOT",
                    "earningType": "0",
                    "realizedEarnings": "0"
                }
            ],
            "purchasedTime": "1712908001000",
            "redeemedTime": "1712914294000",
            "tag": ""
       }
    ]
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ordId | String | Order ID |
| ccy | String | Currency |
| productId | String | Product ID |
| state | String | Order state |
| protocol | String | Protocol |
| protocolType | String | Protocol type |
| term | String | Protocol term |
| apy | String | Estimated APY |
| investData | Array | Investment data |
| earningData | Array | Earning data |
| purchasedTime | String | Order purchased time |
| redeemedTime | String | Order redeemed time |
| tag | String | Order tag |
