### Get instruments
Retrieve available instruments info of current account.

Rate Limit: 20 requests per 2 seconds
Rate limit rule: User ID + Instrument Type
Permission: Read
HTTP Request
```
GET /api/v5/account/instruments
```

Request Example

```
GET /api/v5/account/instruments?instType=SPOT
```

Request Parameters
| Parameter  | Type   | Required    | Description                                                      |
| ---------- | ------ | ----------- | ---------------------------------------------------------------- |
| instType   | String | Yes         | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION             |
| uly        | String | Conditional | Underlying; required if instType=OPTION (or instFamily required) |
| instFamily | String | Conditional | Instrument family; required if instType=OPTION (or uly required) |
| instId     | String | No          | Instrument ID                                                    |

Request Example:

```
GET /api/v5/account/instruments?instType=SPOT
```


Response Example:

```json
{
    "code": "0",
    "data": [
        {
            "auctionEndTime": "",
            "baseCcy": "BTC",
            "ctMult": "",
            "ctType": "",
            "ctVal": "",
            "ctValCcy": "",
            "contTdSwTime": "1704876947000",
            "expTime": "",
            "futureSettlement": false,
            "instFamily": "",
            "instId": "BTC-EUR",
            "instType": "SPOT",
            "lever": "",
            "listTime": "1704876947000",
            "lotSz": "0.00000001",
            "maxIcebergSz": "9999999999.0000000000000000",
            "maxLmtAmt": "1000000",
            "maxLmtSz": "9999999999",
            "maxMktAmt": "1000000",
            "maxMktSz": "1000000",
            "maxStopSz": "1000000",
            "maxTriggerSz": "9999999999.0000000000000000",
            "maxTwapSz": "9999999999.0000000000000000",
            "minSz": "0.00001",
            "optType": "",
            "openType": "call_auction",
            "quoteCcy": "EUR",
            "tradeQuoteCcyList": [
                "EUR"
            ],
            "settleCcy": "",
            "state": "live",
            "ruleType": "normal",
            "stk": "",
            "tickSz": "1",
            "uly": ""
        }
    ],
    "msg": ""
}
```


Response Parameters
| Parameter           | Type               | Description                                                                 |
|--------------------|------------------|-----------------------------------------------------------------------------|
| instType           | String            | Instrument type                                                              |
| instId             | String            | Instrument ID, e.g. BTC-USD-SWAP                                            |
| uly                | String            | Underlying, e.g. BTC-USD. Only applicable to MARGIN/FUTURES/SWAP/OPTION     |
| instFamily         | String            | Instrument family, e.g. BTC-USD. Only applicable to MARGIN/FUTURES/SWAP/OPTION |
| baseCcy            | String            | Base currency, e.g. BTC in BTC-USDT. Only applicable to SPOT/MARGIN         |
| quoteCcy           | String            | Quote currency, e.g. USDT in BTC-USDT. Only applicable to SPOT/MARGIN       |
| settleCcy          | String            | Settlement and margin currency, e.g. BTC. Only applicable to FUTURES/SWAP/OPTION |
| ctVal              | String            | Contract value. Only applicable to FUTURES/SWAP/OPTION                       |
| ctMult             | String            | Contract multiplier. Only applicable to FUTURES/SWAP/OPTION                  |
| ctValCcy           | String            | Contract value currency. Only applicable to FUTURES/SWAP/OPTION              |
| optType            | String            | Option type, C: Call P: Put. Only applicable to OPTION                       |
| stk                | String            | Strike price. Only applicable to OPTION                                      |
| listTime           | String            | Listing time, Unix timestamp in milliseconds                                 |
| auctionEndTime     | String            | End time of call auction, Unix timestamp in milliseconds. Only for SPOT call auctions; return "" otherwise (deprecated) |
| contTdSwTime       | String            | Continuous trading switch time. Only for SPOT/MARGIN with call auction/prequote; return "" otherwise |
| openType           | String            | Open type: fix_price: fix price opening, pre_quote: pre-quote, call_auction: call auction. Only applicable to SPOT/MARGIN; return "" otherwise |
| expTime            | String            | Expiry time. Applicable to SPOT/MARGIN/FUTURES/SWAP/OPTION. For FUTURES/OPTION, it is natural delivery/exercise time |
| lever              | String            | Max leverage. Not applicable to SPOT, OPTION                                 |
| tickSz             | String            | Tick size, e.g. 0.0001. For OPTION, minimum tick size among tick bands      |
| lotSz              | String            | Lot size. Number of contracts for derivatives; quantity in base currency for SPOT/MARGIN |
| minSz              | String            | Minimum order size. Number of contracts for derivatives; quantity in base currency for SPOT/MARGIN |
| ctType             | String            | Contract type: linear (linear contract), inverse (inverse contract). Only for FUTURES/SWAP |
| state              | String            | Instrument status: live, suspend, preopen, test                               |
| ruleType           | String            | Trading rule types: normal, pre_market                                       |
| maxLmtSz           | String            | Max order quantity of a single limit order                                   |
| maxMktSz           | String            | Max order quantity of a single market order                                  |
| maxLmtAmt          | String            | Max USD amount for a single limit order                                      |
| maxMktAmt          | String            | Max USD amount for a single market order. Only applicable to SPOT/MARGIN     |
| maxTwapSz          | String            | Max order quantity of a single TWAP order. Minimum order = minSz*2           |
| maxIcebergSz       | String            | Max order quantity of a single iceberg order                                  |
| maxTriggerSz       | String            | Max order quantity of a single trigger order                                  |
| maxStopSz          | String            | Max order quantity of a single stop market order                              |
| futureSettlement   | Boolean           | Whether daily settlement for expiry feature is enabled. Applicable to FUTURES cross |
| tradeQuoteCcyList  | Array of strings  | List of quote currencies available for trading, e.g. ["USD", "USDC"]         |

listTime and contTdSwTime
For spot symbols listed through a call auction or pre-open, listTime represents the start time of the auction or pre-open, and contTdSwTime indicates the end of the auction or pre-open and the start of continuous trading. For other scenarios, listTime will mark the beginning of continuous trading, and contTdSwTime will return an empty value "".

 state
The state will always change from `preopen` to `live` when the listTime is reached.
When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument will not be available.
