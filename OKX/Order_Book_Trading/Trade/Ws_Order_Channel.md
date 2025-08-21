### WS / Order channel

Retrieve order information via WebSocket. Data will not be pushed when first subscribed. Data will only be pushed when there are order updates.

URL Path  
`/ws/v5/private` (required login)

Request Example : Single

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "orders",
      "instType": "FUTURES",
      "instId": "BTC-USD-200329"
    }
  ]
}
```

Request Example

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "orders",
      "instType": "FUTURES",
      "instFamily": "BTC-USD"
    }
  ]
}
```

Request Parameters

| Parameter  | Type   | Required | Description                                        |
|------------|--------|----------|----------------------------------------------------|
| id         | String | No       | Unique identifier of the message                   |
| op         | String | Yes      | Operation: subscribe or unsubscribe                |
| args       | Array  | Yes      | List of subscribed channels                        |

args Structure:

| Parameter    | Type   | Required | Description                 |
|--------------|--------|----------|-----------------------------|
| channel      | String | Yes      | Channel name: orders        |
| instType     | String | Yes      | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION, ANY |
| instFamily   | String | No       | Instrument family (optional)|
| instId       | String | No       | Instrument ID (optional)    |


Successful Response Example : Single

```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "orders",
    "instType": "FUTURES",
    "instId": "BTC-USD-200329"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"orders\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

Response parameters

| Parameter   | Type   | Required | Description      |
|-------------|--------|----------|------------------|
| id          | String | No       | Unique identifier of the message|
| event       | String | Yes      | Event: subscribe, unsubscribe, error|
| arg         | Object | No       | Subscribed channel              |
| code        | String | No       | Error code                      |
| msg         | String | No       | Error message                   |
| connId      | String | Yes      | WebSocket connection ID         |

arg Structure:

| Parameter    | Type   | Required | Description      |
|--------------|--------|----------|------------------|
| channel      | String | Yes      | Channel name     |
| instType     | String | Yes      | Instrument type  |
| instFamily   | String | No       | Instrument family|
| instId       | String | No       | Instrument ID    |

***

Push Data Example

```
{
    "arg": {
        "channel": "orders",
        "instType": "SPOT",
        "instId": "BTC-USDT",
        "uid": "614488474791936"
    },
    "data": [
        {
            "accFillSz": "0.001",
            "algoClOrdId": "",
            "algoId": "",
            "amendResult": "",
            "amendSource": "",
            "avgPx": "31527.1",
            "cancelSource": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "code": "0",
            "cTime": "1654084334977",
            "execType": "M",
            "fee": "-0.02522168",
            "feeCcy": "USDT",
            "fillFee": "-0.02522168",
            "fillFeeCcy": "USDT",
            "fillNotionalUsd": "31.50818374",
            "fillPx": "31527.1",
            "fillSz": "0.001",
            "fillPnl": "0.01",
            "fillTime": "1654084353263",
            "fillPxVol": "",
            "fillPxUsd": "",
            "fillMarkVol": "",
            "fillFwdPx": "",
            "fillMarkPx": "",
            "fillIdxPx": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "lever": "0",
            "msg": "",
            "notionalUsd": "31.50818374",
            "ordId": "452197707845865472",
            "ordType": "limit",
            "pnl": "0",
            "posSide": "",
            "px": "31527.1",
            "pxUsd":"",
            "pxVol":"",
            "pxType":"",
            "quickMgnType": "",
            "rebate": "0",
            "rebateCcy": "BTC",
            "reduceOnly": "false",
            "reqId": "",
            "side": "sell",
            "attachAlgoClOrdId": "",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "last",
            "source": "",
            "state": "filled",
            "stpId": "",
            "stpMode": "",
            "sz": "0.001",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "last",
            "attachAlgoOrds": [],
            "tradeId": "242589207",
            "tradeQuoteCcy": "USDT",
            "lastPx": "38892.2",
            "uTime": "1654084353264",
            "isTpLimit": "false",
            "linkedAlgoOrd": {
                "algoId": ""
            }
        }
    ]
}
```

Push data parameters

| Parameter     | Type    | Description        |
|---------------|---------|--------------------|
| arg           | Object  | Subscribed channel |
| data          | Array   | Subscribed data    |

arg Structure:

| Parameter     | Type   | Description        |
|---------------|--------|--------------------|
| channel       | String | Channel name       |
| uid           | String | User Identifier    |
| instType      | String | Instrument type    |
| instFamily    | String | Instrument family  |
| instId        | String | Instrument ID      |

data Structure (many fields):

| Field         | Type    | Description                                 |
|---------------|---------|---------------------------------------------|
| instType      | String  | Instrument type                             |
| instId        | String  | Instrument ID                               |
| tgtCcy        | String  | Order quantity unit setting (base_ccy/quote_ccy)|
| ccy           | String  | Margin currency                             |
| ordId         | String  | Order ID                                    |
| clOrdId       | String  | Client Order ID                             |
| tag           | String  | Order tag                                   |
| px            | String  | Price                                       |
| pxUsd         | String  | Options price in USD                        |
| pxVol         | String  | Implied volatility                          |
| pxType        | String  | Price type                                  |
| sz            | String  | Order quantity                              |
| notionalUsd   | String  | Notional value in USD                       |
| ordType       | String  | Order type                                  |
| side          | String  | buy/sell                                    |
| posSide       | String  | Position side: net/long/short               |
| tdMode        | String  | Trade mode (cross/isolated/cash)            |
| fillPx        | String  | Last filled price                           |
| tradeId       | String  | Last trade ID                               |
| fillSz        | String  | Last filled quantity                        |
| fillPnl       | String  | Last filled profit and loss                 |
| fillTime      | String  | Last filled time                            |
| fillFee       | String  | Fee or rebate amount                        |
| fillFeeCcy    | String  | Fee or rebate currency                      |
| fillPxVol     | String  | Implied volatility when filled (options)    |
| fillPxUsd     | String  | Options price when filled (options)         |
| fillMarkVol   | String  | Mark volatility when filled (options)       |
| fillFwdPx     | String  | Forward price when filled (options)         |
| fillMarkPx    | String  | Mark price when filled                      |
| fillIdxPx     | String  | Index price at moment of trade execution    |
| execType      | String  | Taker (T) or Maker (M)                      |
| accFillSz     | String  | Accumulated fill quantity                   |
| fillNotionalUsd|String  | Filled notional value in USD                |
| avgPx         | String  | Average filled price                        |
| state         | String  | Order state                                 |
| lever         | String  | Leverage                                    |
| attachAlgoClOrdId|String| Client-supplied Algo ID (TP/SL)             |
| tpTriggerPx   | String  | TP trigger price                            |
| tpTriggerPxType| String | TP trigger price type                       |
| tpOrdPx       | String  | TP order price                              |
| slTriggerPx   | String  | SL trigger price                            |
| slTriggerPxType| String | SL trigger price type                       |
| slOrdPx       | String  | SL order price                              |
| attachAlgoOrds| Array   | TP/SL info attached on order                |
| linkedAlgoOrd | Object  | Linked SL order details                     |
| stpId         | String  | Self trade prevention ID                    |
| stpMode       | String  | Self trade prevention mode                  |
| feeCcy        | String  | Fee currency                                |
| fee           | String  | Fee and rebate                              |
| rebateCcy     | String  | Rebate currency                             |
| rebate        | String  | Rebate accumulated amount                   |
| pnl           | String  | Profit and loss                             |
| source        | String  | Order source                                |
| cancelSource  | String  | Order cancellation source                   |
| amendSource   | String  | Order amendation source                     |
| category      | String  | Category                                    |
| isTpLimit     | String  | TP limit order (true/false)                 |
| uTime         | String  | Update time (ms)                            |
| cTime         | String  | Creation time (ms)                          |
| reqId         | String  | Client request ID for amendment             |
| amendResult   | String  | Amendment result                            |
| reduceOnly    | String  | Reduce only (true/false)                    |
| quickMgnType  | String  | Quick margin type                           |
| algoClOrdId   | String  | Client-supplied Algo ID                     |
| algoId        | String  | Algo ID                                     |
| lastPx        | String  | Last price                                  |
| code          | String  | Error Code                                  |
| msg           | String  | Error Message                               |
| tradeQuoteCcy | String  | The quote currency used for trading         |
