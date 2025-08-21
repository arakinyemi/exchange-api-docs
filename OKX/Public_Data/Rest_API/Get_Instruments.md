### Get instruments
Retrieve a list of instruments with open contracts.

**Rate Limit:** 20 requests per 2 seconds  
**Rate limit rule:** IP + Instrument Type

HTTP Request
```
GET /api/v5/public/instruments
```

Request Example
```
GET /api/v5/public/instruments?instType=SPOT
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instType | String | Yes | Instrument type<br>SPOT |
| instId | String | No | Instrument ID |

Response Example
```
{
    "code":"0",
    "msg":"",
    "data":[
      {
            "alias": "",
            "auctionEndTime": "",
            "baseCcy": "BTC",
            "category": "1",
            "ctMult": "",
            "ctType": "",
            "ctVal": "",
            "ctValCcy": "",
            "expTime": "",
            "futureSettlement": false,
            "instFamily": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "lever": "10",
            "listTime": "1606468572000",
            "lotSz": "0.00000001",
            "maxIcebergSz": "9999999999.0000000000000000",
            "maxLmtAmt": "1000000",
            "maxLmtSz": "9999999999",
            "maxMktAmt": "1000000",
            "maxMktSz": "",
            "maxStopSz": "",
            "maxTriggerSz": "9999999999.0000000000000000",
            "maxTwapSz": "9999999999.0000000000000000",
            "minSz": "0.00001",
            "optType": "",
            "openType": "call_auction",
            "quoteCcy": "USDT",
            "settleCcy": "",
            "state": "live",
            "stk": "",
            "tickSz": "0.1",
            "uly": ""
        }
    ]
}
```

Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instType | String | Instrument type |
| instId | String | Instrument ID, e.g. BTC-USDT |
| uly | String | Underlying, e.g. BTC-USD<br>Only applicable to FUTURES/SWAP/OPTION |
| instFamily | String | Instrument family, e.g. BTC-USD<br>Only applicable to FUTURES/SWAP/OPTION |
| category | String | Currency category. Note: this parameter is already deprecated |
| baseCcy | String | Base currency, e.g. BTC inBTC-USDT<br>Only applicable to SPOT/MARGIN |
| quoteCcy | String | Quote currency, e.g. USDT in BTC-USDT<br>Only applicable to SPOT/MARGIN |
| settleCcy | String | Settlement and margin currency, e.g. BTC<br>Only applicable to FUTURES/SWAP/OPTION |
| ctVal | String | Contract value<br>Only applicable to FUTURES/SWAP/OPTION |
| ctMult | String | Contract multiplier<br>Only applicable to FUTURES/SWAP/OPTION |
| ctValCcy | String | Contract value currency<br>Only applicable to FUTURES/SWAP/OPTION |
| optType | String | Option type, C: Call P: put<br>Only applicable to OPTION |
| stk | String | Strike price<br>Only applicable to OPTION |
| listTime | String | Listing time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| auctionEndTime | String | The end time of call auction, Unix timestamp format in milliseconds, e.g. 1597026383085<br>Only applicable to SPOT that are listed through call auctions, return "" in other cases (deprecated, use contTdSwTime) |
| contTdSwTime | String | Continuous trading switch time. The switch time from call auction, prequote to continuous trading, Unix timestamp format in milliseconds. e.g. 1597026383085.<br>Only applicable to SPOT/MARGIN that are listed through call auction or prequote, return "" in other cases. |
| openType | String | Open type<br>fix_price: fix price opening<br>pre_quote: pre-quote<br>call_auction: call auction<br>Only applicable to SPOT/MARGIN, return "" for all other business lines |
| expTime | String | Expiry time<br>Applicable to SPOT/MARGIN/FUTURES/SWAP/OPTION. For FUTURES/OPTION, it is natural delivery/exercise time. It is the instrument offline time when there is SPOT/MARGIN/FUTURES/SWAP/ manual offline. Update once change. |
| lever | String | Max Leverage,<br>Not applicable to SPOT, OPTION |
| tickSz | String | Tick size, e.g. 0.0001<br>For Option, it is minimum tickSz among tick band, please use "Get option tick bands" if you want get option tickBands. |
| lotSz | String | Lot size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| minSz | String | Minimum order size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| ctType | String | Contract type<br>linear: linear contract<br>inverse: inverse contract<br>Only applicable to FUTURES/SWAP |
| alias | String | Alias<br>this_week<br>next_week<br>this_month<br>next_month<br>quarter<br>next_quarter<br>third_quarter<br>Only applicable to FUTURES<br>Not recommended for use, users are encouraged to rely on the expTime field to determine the delivery time of the contract |
| state | String | Instrument status<br>live<br>suspend<br>preopen e.g. Futures and options contracts rollover from generation to trading start; certain symbols before they go live<br>test: Test pairs, can't be traded |
| maxLmtSz | String | The maximum order quantity of a single limit order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| maxMktSz | String | The maximum order quantity of a single market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in USDT. |
| maxLmtAmt | String | Max USD amount for a single limit order |
| maxMktAmt | String | Max USD amount for a single market order<br>Only applicable to SPOT/MARGIN |
| maxTwapSz | String | The maximum order quantity of a single TWAP order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| maxIcebergSz | String | The maximum order quantity of a single iceBerg order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| maxTriggerSz | String | The maximum order quantity of a single trigger order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| maxStopSz | String | The maximum order quantity of a single stop market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in USDT. |
| futureSettlement | Boolean | Whether daily settlement for expiry feature is enabled<br>Applicable to FUTURES cross |

#### Notes on listTime and contTdSwTime
For spot symbols listed through a call auction or pre-open, listTime represents the start time of the auction or pre-open, and contTdSwTime indicates the end of the auction or pre-open and the start of continuous trading. For other scenarios, listTime will mark the beginning of continuous trading, and contTdSwTime will return an empty value "".

#### Notes on state
The state will always change from `preopen` to `live` when the listTime is reached.
When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument will not be available.
