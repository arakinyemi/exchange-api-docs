### Get / Order List

Retrieve all incomplete orders under the current account.  
Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

HTTP Request  
`GET /api/v5/trade/orders-pending`

| Parameter  | Type    | Required | Description                              |
|------------|---------|----------|------------------------------------------|
| uly        | String  | No       | Underlying, e.g. BTC-USD (only applicable to MARGIN/FUTURES/SWAP/OPTION) |
| instFamily | String  | No       | Instrument family, e.g. BTC-USD (only applicable to MARGIN/FUTURES/SWAP/OPTION) |
| baseCcy    | String  | No       | Base currency, e.g. BTC in BTC-USDT (only applicable to SPOT/MARGIN) |
| quoteCcy   | String  | No       | Quote currency, e.g. USDT in BTC-USDT (only applicable to SPOT/MARGIN) |
| settleCcy  | String  | No       | Settlement and margin currency, e.g. BTC (only applicable to FUTURES/SWAP/OPTION) |
| ctVal      | String  | No       | Contract value (only applicable to FUTURES/SWAP/OPTION) |
| ctMult     | String  | No       | Contract multiplier (only applicable to FUTURES/SWAP/OPTION) |
| ctValCcy   | String  | No       | Contract value currency (only applicable to FUTURES/SWAP/OPTION) |
| optType    | String  | No       | Option type, C: Call, P: put (only applicable to OPTION) |
| stk        | String  | No       | Strike price |

Request Example  
`GET /api/v5/trade/orders-pending?ordType=post_only,fok,ioc&instType=SPOT`

Request Parameters

| Parameter | Type   | Required | Description                                                          |
|-----------|--------|----------|----------------------------------------------------------------------|
| instType  | String | No       | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION                 |
| uly       | String | No       | Underlying (Applicable to FUTURES/SWAP/OPTION)                       |
| instFamily| String | No       | Instrument family (Applicable to FUTURES/SWAP/OPTION)                |
| instId    | String | No       | Instrument ID, e.g. BTC-USD-200927                                  |
| ordType   | String | No       | Order type: market, limit, post_only, fok, ioc, optimal_limit_ioc, mmp, mmp_and_post_only, op_fok |
| state     | String | No       | State: live, partially_filled                                        |
| after     | String | No       | Pagination of data to return records earlier than the requested ordId |
| before    | String | No       | Pagination of data to return records newer than the requested ordId  |
| limit     | String | No       | Number of results per request. Max 100; default 100                  |

Response Example
```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0",
            "algoClOrdId": "",
            "algoId": "",
            "attachAlgoClOrdId": "",
            "attachAlgoOrds": [],
            "avgPx": "",
            "cTime": "1724733617998",
            "cancelSource": "",
            "cancelSourceReason": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "fee": "0",
            "feeCcy": "BTC",
            "fillPx": "",
            "fillSz": "0",
            "fillTime": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "isTpLimit": "false",
            "lever": "",
            "linkedAlgoOrd": {
                "algoId": ""
            },
            "ordId": "1752588852617379840",
            "ordType": "post_only",
            "pnl": "0",
            "posSide": "net",
            "px": "13013.5",
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
            "state": "live",
            "stpId": "",
            "stpMode": "cancel_maker",
            "sz": "0.001",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "tradeId": "",
            "tradeQuoteCcy": "USDT",
            "uTime": "1724733617998"
        }
    ],
    "msg": ""
}
```

Response Parameters

| Parameter      | Type    | Description                                                                                          |
|----------------|---------|-----------------------------------------------------------------------------------------------------|
| instType       | String  | Instrument type                                                                                     |
| instId         | String  | Instrument ID                                                                                       |
| tgtCcy         | String  | Order quantity unit setting for sz: base_ccy or quote_ccy (Applicable to SPOT Market Orders)       |
| ccy            | String  | Margin currency (Applicable to all isolated MARGIN orders and cross MARGIN orders in Futures mode)  |
| ordId          | String  | Order ID                                                                                           |
| clOrdId        | String  | Client Order ID as assigned by the client                                                         |
| tag            | String  | Order tag                                                                                         |
| px             | String  | Price (For options, use coin as unit e.g. BTC, ETH)                                               |
| pxUsd          | String  | Options price in USD (Only applicable to options; return "" for other instrument types)           |
| pxVol          | String  | Implied volatility of the options order (Only applicable to options; return "" for other types)   |
| pxType         | String  | Price type of options: px, pxVol, pxUsd                                                           |
| sz             | String  | Quantity to buy or sell                                                                            |
| pnl            | String  | Profit and loss, 0 if conditions don’t apply                                                     |
| ordType        | String  | Order type                                                                                        |
| side           | String  | Order side                                                                                        |
| posSide        | String  | Position side                                                                                    |
| tdMode         | String  | Trade mode                                                                                       |
| accFillSz      | String  | Accumulated fill quantity                                                                        |
| fillPx         | String  | Last filled price                                                                                |
| tradeId        | String  | Last trade ID                                                                                   |
| fillSz         | String  | Last filled quantity                                                                            |
| fillTime       | String  | Last filled time                                                                               |
| avgPx          | String  | Average filled price                                                                           |
| state          | String  | State: live, partially_filled                                                                 |
| lever          | String  | Leverage (only applicable to MARGIN/FUTURES/SWAP)                                             |
| attachAlgoClOrdId | String | Client-supplied Algo ID when placing order attaching TP/SL                                   |
| tpTriggerPx    | String  | Take-profit trigger price                                                                      |
| tpTriggerPxType| String  | Take-profit trigger price type: last, index, mark                                             |
| tpOrdPx       | String   | Take-profit order price                                                                       |
| slTriggerPx    | String  | Stop-loss trigger price                                                                        |
| slTriggerPxType| String  | Stop-loss trigger price type: last, index, mark                                              |
| slOrdPx       | String   | Stop-loss order price                                                                         |
| attachAlgoOrds | Array    | TP/SL information attached when placing order (see subfields)                                |
| linkedAlgoOrd  | Object   | Linked SL order detail (for OCO orders with TP limit order)                                   |
| stpId         | String   | Self trade prevention ID (deprecated)                                                         |
| stpMode       | String   | Self trade prevention mode                                                                    |
| feeCcy        | String   | Fee currency                                                                                  |
| fee           | String   | Fee and rebate                                                                               |
| rebateCcy     | String   | Rebate currency                                                                             |
| source        | String   | Order source codes: 6, 7, 13, 25, 34                                                         |
| rebate        | String   | Rebate amount (spot and margin only)                                                        |
| category      | String   | Category: normal                                                                            |
| reduceOnly    | String   | Whether order can only reduce position size (true/false)                                    |
| quickMgnType  | String   | Quick Margin type (only for Quick Margin Mode)                                            |
| algoClOrdId   | String   | Client-supplied Algo ID (if any)                                                          |
| algoId        | String   | Algo ID (if any)                                                                          |
| isTpLimit     | String   | Whether it is TP limit order (true/false)                                                |
| uTime         | String   | Update time in Unix timestamp (ms)                                                     |
| cTime         | String   | Creation time in Unix timestamp (ms)                                                   |
| cancelSource  | String   | Code of cancellation source                                                           |
| cancelSourceReason | String | Reason for cancellation                                                               |
| tradeQuoteCcy | String   | Quote currency used for trading                                                       |

***
