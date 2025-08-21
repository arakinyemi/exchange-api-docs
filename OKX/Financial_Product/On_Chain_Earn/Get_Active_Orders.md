### GET / Active orders
**Rate Limit**: 3 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/finance/staking-defi/orders-active`

Request Example
```
GET /api/v5/finance/staking-defi/orders-active
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| productId | String | No | Product ID |
| protocolType | String | No | Protocol type |
| ccy | String | No | Investment currency |
| state | String | No | Order state |

Response Example
```
{
    "code": "0",
    "data": [
        {
            "ordId": "2413499",
            "ccy": "DOT",
            "productId": "101",
            "state": "1",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1014",
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
                    "earnings": "0.10615025"
                }
            ],
            "purchasedTime": "1729839328000",
            "tag": "",
            "estSettlementTime": "",
            "cancelRedemptionDeadline": "",
            "fastRedemptionData": []
        }
    ],
    "msg": ""
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
| tag | String | Order tag |
