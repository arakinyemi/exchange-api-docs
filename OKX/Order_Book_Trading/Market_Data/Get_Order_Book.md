### GET / Order book

Retrieve order book of the instrument. The data will be updated once every 50 milliseconds.  
Best ask price may be lower than the best bid price during the pre-open period.  
This endpoint does not return data immediately. Instead, it returns the latest data once the server-side cache has been updated.

**Rate Limit:** 40 requests per 2 seconds  
**Rate limit rule:** IP

**HTTP Request**  
`GET /api/v5/market/books`

#### Request Example

```
GET /api/v5/market/books?instId=BTC-USDT
```

#### Request Parameters

| Parameter | Type   | Required | Description                                  |
| --------- | ------ | -------- | -------------------------------------------- |
| instId    | String | Yes      | Instrument ID, e.g. BTC-USDT                  |
| sz        | String | No       | Order book depth per side. Max 400 (e.g. 400 bids + 400 asks). Default is 1 depth |

#### Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "asks": [
                [
                    "41006.8",
                    "0.60038921",
                    "0",
                    "1"
                ]
            ],
            "bids": [
                [
                    "41006.3",
                    "0.30178218",
                    "0",
                    "2"
                ]
            ],
            "ts": "1629966436396"
        }
    ]
}
```

#### Response Parameters

| Parameter | Type           | Description                                                                            |
|-----------|----------------|----------------------------------------------------------------------------------------|
| asks      | Array of Arrays | Order book on sell side                                                                |
| bids      | Array of Arrays | Order book on buy side                                                                 |
| ts        | String          | Order book generation time                                                             |

An example of the asks and bids array values: `["411.8", "10", "0", "4"]`
- `"411.8"` is the depth price
- `"10"` is the quantity at the price (number of contracts for derivatives, quantity in base currency for Spot and Spot Margin)
- `"0"` is part of a deprecated feature and always `"0"`
- `"4"` is the number of orders at the price

The order book data will be updated around once a second during the call auction.

***
