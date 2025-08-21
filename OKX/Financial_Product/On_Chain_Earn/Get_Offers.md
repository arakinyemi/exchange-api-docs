### GET / Offers
**Rate Limit**: 3 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/finance/staking-defi/offers`

Request Example
```
GET /api/v5/finance/staking-defi/offers
```

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| productId | String | No | Product ID |
| protocolType | String | No | Protocol type (defi: on-chain earn) |
| ccy | String | No | Investment currency |

Response Example
```
{
    "code": "0",
    "data": [
        {
            "ccy": "DOT",
            "productId": "101",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1767",
            "earlyRedeem": false,
            "state": "purchasable",
            "investData": [
                {
                    "bal": "0",
                    "ccy": "DOT",
                    "maxAmt": "0",
                    "minAmt": "2"
                }
            ],
            "earningData": [
                {
                    "ccy": "DOT",
                    "earningType": "0"
                }
            ],
            "fastRedemptionDailyLimit": "",
            "redeemPeriod": [
                "28D",
                "28D"
            ]
        }
    ],
    "msg": ""
}
```

Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency |
| productId | String | Product ID |
| protocol | String | Protocol |
| protocolType | String | Protocol type |
| term | String | Protocol term (0 for flexible) |
| apy | String | Estimated annualization |
| earlyRedeem | Boolean | Early redemption support |
| state | String | Product state |
| investData | Array | Investment data |
| earningData | Array | Earning data |
| fastRedemptionDailyLimit | String | Fast redemption daily limit |
| redeemPeriod | Array | Redemption period |
