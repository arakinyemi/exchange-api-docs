### Get Exchange List (Public)

Authentication is not required for this public endpoint.

**Rate Limit**: 6 requests per second  
**Rate limit rule**: IP  

HTTP Request
`GET /api/v5/asset/exchange-list`

Request Example
```
GET /api/v5/asset/exchange-list
```

Request Parameters
None

Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "exchId": "did:ethr:0xfeb4f99829a9acdf52979abee87e83addf22a7e1",
        "exchName": "1xbet"
    }
  ]
}
```

Response Parameters

| Parameter | Type   | Description |
|-----------|--------|-------------|
| exchName  | String | Exchange name, e.g. 1xbet |
| exchId    | String | Exchange ID, e.g. did:ethr:0xfeb4f99829a9acdf52979abee87e83addf22a7e1 |
