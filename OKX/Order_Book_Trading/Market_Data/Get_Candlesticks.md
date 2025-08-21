### GET / Candlesticks

Retrieve the candlestick charts. This endpoint can retrieve the latest 1,440 data entries.  
Charts are returned in groups based on the requested bar.

**Rate Limit:** 40 requests per 2 seconds  
**Rate limit rule:** IP

**HTTP Request**  
`GET /api/v5/market/candles`

#### Request Example

```
GET /api/v5/market/candles?instId=BTC-USDT
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                     |
| --------- | ------ | -------- | ----------------------------------------------------------------------------------------------- |
| instId    | String | Yes      | Instrument ID, e.g. BTC-USDT                                                                    |
| bar       | String | No       | Bar size, default is 1m  
e.g. [1m/3m/5m/15m/30m/1H/2H/4H]  
UTC+8 opening price k-line: [6H/12H/1D/2D/3D/1W/1M/3M]  
UTC+0 opening price k-line: [/6Hutc/12Hutc/1Dutc/2Dutc/3Dutc/1Wutc/1Mutc/3Mutc]           |
| after     | String | No       | Pagination: to return records earlier than requested ts                                         |
| before    | String | No       | Pagination: to return records newer than requested ts. Returns latest data if used alone        |
| limit     | String | No       | Number of results per request, max 300. Default 100                                            |

#### Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        [
            "1597026383085",
            "3.721",
            "3.743",
            "3.677",
            "3.708",
            "8422410",
            "22698348.04828491",
            "12698348.04828491",
            "0"
        ],
        [
            "1597026383085",
            "3.731",
            "3.799",
            "3.494",
            "3.72",
            "24912403",
            "67632347.24399722",
            "37632347.24399722",
            "1"
        ]
    ]
}
```

#### Response Parameters

| Parameter    | Type   | Description                                                                                      |
|--------------|--------|------------------------------------------------------------------------------------------------|
| ts           | String | Opening time of the candlestick, Unix timestamp in milliseconds (e.g. 1597026383085)            |
| o            | String | Open price                                                                                    |
| h            | String | Highest price                                                                                 |
| l            | String | Lowest price                                                                                  |
| c            | String | Close price                                                                                   |
| vol          | String | Trading volume (SPOT: quantity in base currency)                                              |
| volCcy       | String | Trading volume (SPOT: quantity in quote currency)                                             |
| volCcyQuote  | String | Trading volume, quantity in quote currency (e.g. unit is USDT for BTC-USDT)                    |
| confirm      | String | State of candlesticks:  
0: K line is uncompleted  
1: K line is completed  

The first candlestick data may be incomplete and should not be polled repeatedly.

***

The data is arranged in an array like: `[ts,o,h,l,c,vol,volCcy,volCcyQuote,confirm]`.  

For the current k-line cycle, when no transaction occurs, the open, high, and close prices default to the previous cycle's close.

***
