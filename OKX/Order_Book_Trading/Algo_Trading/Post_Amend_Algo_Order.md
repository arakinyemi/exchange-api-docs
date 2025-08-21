### POST / Amend algo order

Amend unfilled algo orders (Stop order and Trigger order only).

Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Permission: Trade  

HTTP Request

`POST /api/v5/trade/amend-algos`

Request Example

```
POST /api/v5/trade/amend-algos
{
    "algoId":"2510789768709120",
    "newSz":"2",
    "instId":"BTC-USDT"
}
```

Request Parameters

| Parameter       | Type    | Required    | Description                                         |
|-----------------|---------|-------------|-----------------------------------------------------|
| instId          | String  | Yes         | Instrument ID                                      |
| algoId          | String  | Conditional | Algo ID (Either algoId or algoClOrdId required)  |
| algoClOrdId     | String  | Conditional | Client-supplied Algo ID                            |
| cxlOnFail       | Boolean | No          | Cancel if amendment fails (default false)         |
| reqId           | String  | Conditional | Client Request ID for amendment                    |
| newSz           | String  | Conditional | New quantity after amendment (must be > 0)        |
| newTpTriggerPx  | String  | Conditional | New take-profit trigger price                      |
| newTpOrdPx      | String  | Conditional | New take-profit order price                         |
| newSlTriggerPx  | String  | Conditional | New stop-loss trigger price                         |
| newSlOrdPx      | String  | Conditional | New stop-loss order price                           |
| newTpTriggerPxType | String | Conditional | Take-profit trigger price type                     |
| newSlTriggerPxType | String | Conditional | Stop-loss trigger price type                        |

Response Example

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId":"algo_01",
            "algoId":"2510789768709120",
            "reqId":"po103ux",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

Response Parameters

| Parameter   | Type   | Description                                     |
|-------------|--------|------------------------------------------------|
| algoId      | String | Algo ID                                        |
| algoClOrdId | String | Client-supplied Algo ID                        |
| reqId       | String | Client Request ID for amendment                |
| sCode       | String | Event execution result (0 means success)       |
| sMsg        | String | Rejection message if request unsuccessful      |

***
