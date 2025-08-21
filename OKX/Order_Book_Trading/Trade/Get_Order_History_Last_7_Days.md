### Get / Order history (last 7 days)

Get completed orders which are placed in the last 7 days, including those placed 7 days ago but completed in the last 7 days.  
The incomplete orders that have been canceled are only reserved for 2 hours.  
Rate Limit: 40 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

HTTP Request  
`GET /api/v5/trade/orders-history`

Request Example  
`GET /api/v5/trade/orders-history?ordType=post_only,fok,ioc&instType=SPOT`

Request Parameters

| Parameter | Type    | Required | Description                          |
|-----------|---------|----------|--------------------------------------|
| instType  | String  | yes      | Instrument type (SPOT, MARGIN, SWAP, FUTURES, OPTION) |
| uly       | String  | No       | Underlying (Applicable to FUTURES/SWAP/OPTION)        |
| instFamily| String  | No       | Instrument family (Applicable to FUTURES/SWAP/OPTION) |
| instId    | String  | No       | Instrument ID, e.g. BTC-USDT                            |
| ordType   | String  | No       | Order type (market, limit, post_only, fok, ioc, etc.)  |
| state     | String  | No       | State (canceled, filled, mmp_canceled)                 |
| category  | String  | No       | Category (twap, adl, full_liquidation, etc.)           |
| after     | String  | No       | Pagination for records earlier than given ordId        |
| before    | String  | No       | Pagination for records newer than given ordId          |
| begin     | String  | No       | Begin timestamp cTime (ms)                             |
| end       | String  | No       | End timestamp cTime (ms)                               |
| limit     | String  | No       | Number of results, max 100, default 100                |

Response Example
```
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0.00192834",
            "algoClOrdId": "",
            "algoId": "",
            "attachAlgoClOrdId": "",
            "attachAlgoOrds": [],
            "avgPx": "51858",
            "cTime": "1708587373361",
            "cancelSource": "",
            "cancelSourceReason": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "fee": "-0.00000192834",
            "feeCcy": "BTC",
            "fillPx": "51858",
            "fillSz": "0.00192834",
            "fillTime": "1708587373361",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "lever": "",
            "linkedAlgoOrd": {
                "algoId": ""
            },
            "ordId": "680800019749904384",
            "ordType": "market",
            "pnl": "0",
            "posSide": "",
            "px": "",
            "pxType": "",
            "pxUsd": "",
            "pxVol": "",
            "quickMgnType": "",
            "rebate": "0",
            "rebateCcy": "USDT",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "source": "",
            "state": "filled",
            "stpId": "",
            "stpMode": "",
            "sz": "100",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "quote_ccy",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "tradeId": "744876980",
            "tradeQuoteCcy": "USDT",
            "uTime": "1708587373362",
            "isTpLimit": "false"
        }
    ],
    "msg": ""
}
```

Response Parameters  
*(Same as in "GET / Order List" response parameters except `state` field now includes: canceled, filled, mmp_canceled.)*
