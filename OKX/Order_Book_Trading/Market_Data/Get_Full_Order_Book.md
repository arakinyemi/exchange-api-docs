### GET / Full order book

Retrieve order book of the instrument. The data will be updated once a second.  
Best ask price may be lower than the best bid price during the pre-open period.  
This endpoint does not return data immediately. Instead, it returns the latest data once the server-side cache has been updated.

**Rate Limit:** 10 requests per 2 seconds  
**Rate limit rule:** IP

**HTTP Request**  
`GET /api/v5/market/books-full`

#### Request Example

```
GET /api/v5/market/books-full?instId=BTC-USDT&sz=1
```

#### Request Parameters

| Parameter | Type   | Required | Description                                   |
| --------- | ------ | -------- | --------------------------------------------- |
| instId    | String | Yes      | Instrument ID, e.g. BTC-USDT                   |
| sz        | String | No       | Order book depth per side. Max 500 (e.g. 5000 bids + 5000 asks). Default is 1 depth |

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
                    "1"
                ]
            ],
            "bids": [
                [
                    "41006.3",
                    "0.30178218",
                    "2"
                ]
            ],
            "ts": "1629966436396"
        }
    ]
}
```

#### Response Parameters

| Parameter | Type           | Description                                                                       |
|-----------|----------------|-----------------------------------------------------------------------------------|
| asks      | Array of Arrays | Order book on sell side                                                           |
| bids      | Array of Arrays | Order book on buy side                                                            |
| ts        | String          | Order book generation time                                                        |

An example of asks and bids array values: `["411.8", "10", "4"]`  
- `"411.8"` is the depth price  
- `"10"` is the quantity at the price (number of contracts for derivatives, quantity in base currency for Spot and Spot Margin)  
- `"4"` is the number of orders at the price  

The order book data will be updated around once a second during the call auction.

***
