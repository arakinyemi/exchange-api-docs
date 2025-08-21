### Get / Transaction details (last 3 days)

Retrieve recently-filled transaction details in the last 3 days.  
Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

HTTP Request  
`GET /api/v5/trade/fills`

Request Example  
`GET /api/v5/trade/fills`

Request Parameters

| Parameter | Type   | Required | Description                                          |
|-----------|--------|----------|------------------------------------------------------|
| instType  | String | No       | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION|
| uly       | String | No       | Underlying (Applicable to FUTURES/SWAP/OPTION)      |
| instFamily| String | No       | Instrument family (Applicable to FUTURES/SWAP/OPTION)|
| instId    | String | No       | Instrument ID, e.g. BTC-USDT                         |
| ordId     | String | No       | Order ID                                             |
| subType   | String | No       | Transaction type (codes as listed)                   |
| after     | String | No       | Pagination of data to return records earlier than given billId |
| before    | String | No       | Pagination of data to return records newer than given billId  |
| begin     | String | No       | Filter with begin timestamp ts (ms)                  |
| end       | String | No       | Filter with end timestamp ts (ms)                     |
| limit     | String | No       | Number of results per request (max 100, default 100)  |
