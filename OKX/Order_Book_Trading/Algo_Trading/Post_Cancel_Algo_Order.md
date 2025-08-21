### POST / Cancel algo order

Cancel unfilled algo orders. Max 10 orders per request.

Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Permission: Trade  

HTTP Request

`POST /api/v5/trade/cancel-algos`

Request Example

```
POST /api/v5/trade/cancel-algos
[
    {
        "algoId":"198273485",
        "instId":"BTC-USDT"
    },
    {
        "algoId":"198273485",
        "instId":"BTC-USDT"
    }
]
```

Request Parameters

| Parameter | Type   | Required | Description               |
|-----------|--------|----------|---------------------------|
| algoId    | String | Yes      | Algo ID                   |
| instId    | String | Yes      | Instrument ID             |

Response Example

```
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "1836489397437468672",
            "clOrdId": "",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

Response Parameters

| Parameter    | Type   | Description                           |
|--------------|--------|-------------------------------------|
| algoId       | String | Algo ID                            |
| sCode        | String | Event execution code (0 means success) |
| sMsg         | String | Rejection message if unsuccessful   |
| clOrdId      | String | Client Order ID (Deprecated)         |
| algoClOrdId  | String | Client-supplied Algo ID (Deprecated) |
| tag          | String | Order tag (Deprecated)               |

***
