### Instruments channel

**URL Path:** `/ws/v5/public`

Request Example
```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "instruments",
      "instType": "SPOT"
    }
  ]
}
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message<br>Provided by client. It will be returned in response message for identifying the corresponding request.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>subscribe<br>unsubscribe |
| args | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name<br>instruments |
| > instType | String | Yes | Instrument type<br>SPOT |

Successful Response Example
```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "instruments",
    "instType": "SPOT"
  },
  "connId": "a4d3ae55"
}
```

Failure Response Example
```
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"instruments\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

Response parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message |
| event | String | Yes | Event<br>subscribe<br>unsubscribe<br>error |
| arg | Object | No | Subscribed channel |
| > channel | String | Yes | Channel name |
| > instType | String | Yes | Instrument type<br>SPOT |
| code | String | No | Error code |
| msg | String | No | Error message |
| connId | String | Yes | WebSocket connection ID |

Push Data Example
```
{
  "arg": {
    "channel": "instruments",
    "instType": "SPOT"
  },
  "data": [
    {
        "alias": "",
        "baseCcy": "BTC",
        "category": "1",
        "ctMult": "",
        "ctType": "",
        "ctVal": "",
        "ctValCcy": "",
        "contTdSwTime": "1704876947000",
        "expTime": "",
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

Push data parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| arg | Object | Subscribed channel |
| > channel | String | Channel name |
| > instType | String | Instrument type |
| data | Array of objects | Subscribed data |
| > instType | String | Instrument type |
| > instId | String | Instrument ID, e.g. BTC-UST |
| > uly | String | Underlying, e.g. BTC-USD<br>Only applicable to FUTURES/SWAP/OPTION |
| > instFamily | String | Instrument family, e.g. BTC-USD<br>Only applicable to FUTURES/SWAP/OPTION |
| > category | String | Currency category. Note: this parameter is already deprecated |
| > baseCcy | String | Base currency, e.g. BTC in BTC-USDT<br>Only applicable to SPOT/MARGIN |
| > quoteCcy | String | Quote currency, e.g. USDT in BTC-USDT<br>Only applicable to SPOT/MARGIN |
| > settleCcy | String | Settlement and margin currency, e.g. BTC<br>Only applicable to FUTURES/SWAP/OPTION |
| > ctVal | String | Contract value |
| > ctMult | String | Contract multiplier |
| > ctValCcy | String | Contract value currency |
| > optType | String | Option type<br>C: Call<br>P: Put<br>Only applicable to OPTION |
| > stk | String | Strike price<br>Only applicable to OPTION |
| > listTime | String | Listing time |
| > auctionEndTime | String | The end time of call auction, Unix timestamp format in milliseconds, e.g. 1597026383085<br>Only applicable to SPOT that are listed through call auctions, return "" in other cases (deprecated, use contTdSwTime) |
| > contTdSwTime | String | Continuous trading switch time. The switch time from call auction, prequote to continuous trading, Unix timestamp format in milliseconds. e.g. 1597026383085.<br>Only applicable to SPOT/MARGIN that are listed through call auction or prequote, return "" in other cases. |
| > openType | String | Open type<br>fix_price: fix price opening<br>pre_quote: pre-quote<br>call_auction: call auction<br>Only applicable to SPOT/MARGIN, return "" for all other business lines |
| > expTime | String | Expiry time<br>Applicable to SPOT/MARGIN/FUTURES/SWAP/OPTION. For FUTURES/OPTION, it is the delivery/exercise time. It can also be the delisting time of the trading instrument. Update once change. |
| > lever | String | Max Leverage<br>Not applicable to SPOT/OPTION, used to distinguish between MARGIN and SPOT. |
| > tickSz | String | Tick size, e.g. 0.0001<br>For Option, it is minimum tickSz among tick band. |
| > lotSz | String | Lot size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency |
| > minSz | String | Minimum order size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency |
| > ctType | String | Contract type<br>linear: linear contract<br>inverse: inverse contract<br>Only applicable to FUTURES/SWAP |
| > alias | String | Alias<br>this_week<br>next_week<br>this_month<br>next_month<br>quarter<br>next_quarter<br>Only applicable to FUTURES<br>Not recommended for use, users are encouraged to rely on the expTime field to determine the delivery time of the contract |
| > state | String | Instrument status<br>live<br>suspend<br>expired<br>preopen. e.g. There will be preopen before the Futures and Options new contracts state is live.<br>test: Test pairs, can't be traded |
| > maxLmtSz | String | The maximum order quantity of a single limit order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| > maxMktSz | String | The maximum order quantity of a single market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in USDT. |
| > maxTwapSz | String | The maximum order quantity of a single TWAP order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| > maxIcebergSz | String | The maximum order quantity of a single iceBerg order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| > maxTriggerSz | String | The maximum order quantity of a single trigger order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| > maxStopSz | String | The maximum order quantity of a single stop market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in USDT. |

#### Notes
Instrument status will trigger pushing of incremental data from instruments channel. When a new contract is going to be listed, the instrument data of the new contract will be available with status preopen. When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument status will be changed to expired.

#### Notes on listTime and contTdSwTime
For spot symbols listed through a call auction or pre-open, listTime represents the start time of the auction or pre-open, and contTdSwTime indicates the end of the auction or pre-open and the start of continuous trading. For other scenarios, listTime will mark the beginning of continuous trading, and contTdSwTime will return an empty value "".

---
