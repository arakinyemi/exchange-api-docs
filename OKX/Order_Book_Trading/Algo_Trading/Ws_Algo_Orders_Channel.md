### WS / Algo orders channel

Retrieve algo orders (trigger, OCO, conditional). Data pushed on updates only.

URL Path  
`/ws/v5/business` (required login)

Request Example

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "orders-algo",
      "instType": "ANY"
    }
  ]
}
```

Request Parameters

| Parameter | Type   | Required | Description                           |
|-----------|--------|----------|-------------------------------------|
| id        | String | No       | Unique identifier                   |
| op        | String | Yes      | Operation (subscribe/unsubscribe)   |
| args      | Array  | Yes      | List of subscribed channels          |

args structure:

| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| channel   | String | Yes      | Channel name: orders-algo |
| instType  | String | Yes      | Instrument type   |
| instId    | String | No       | Instrument ID     |

Successful Response Example

```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "orders-algo",
    "instType": "ANY"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"orders-algo\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

Response Parameters

| Parameter | Type   | Required | Description                  |
|-----------|--------|----------|------------------------------|
| id        | String | No       | Unique identifier             |
| event     | String | Yes      | Event: subscribe/unsubscribe/error |
| arg       | Object | No       | Subscribed channel content    |
| code      | String | No       | Error code                   |
| msg       | String | No       | Error message                |
| connId    | String | Yes      | WebSocket connection ID      |

arg structure:

| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| channel   | String | Yes      | Channel name      |
| uid       | String | No       | User Identifier   |
| instType  | String | No       | Instrument type   |
| instFamily| String | No       | Instrument family |
| instId    | String | No       | Instrument ID     |

Push Data Example

```
{
    "arg": {
        "channel": "orders-algo",
        "uid": "77982378738415879",
        "instType": "ANY"
    },
    "data": [{
        "actualPx": "0",
        "actualSide": "",
        "actualSz": "0",
        "algoClOrdId": "",
        "algoId": "581878926302093312",
        "attachAlgoOrds": [],
        "amendResult": "",
        "cTime": "1685002746818",
        "ccy": "",
        "clOrdId": "",
        "closeFraction": "",
        "failCode": "",
        "instId": "BTC-USDC",
        "instType": "SPOT",
        "last": "26174.8",
        "lever": "0",
        "notionalUsd": "11.0",
        "ordId": "",
        "ordIdList": [],
        "ordPx": "",
        "ordType": "conditional",
        "posSide": "",
        "quickMgnType": "",
        "reduceOnly": "false",
        "reqId": "",
        "side": "buy",
        "slOrdPx": "",
        "slTriggerPx": "",
        "slTriggerPxType": "",
        "state": "live",
        "sz": "11",
        "tag": "",
        "tdMode": "cross",
        "tgtCcy": "quote_ccy",
        "tpOrdPx": "-1",
        "tpTriggerPx": "1",
        "tpTriggerPxType": "last",
        "triggerPx": "",
        "triggerTime": "",
        "amendPxOnTriggerType": "0"
    }]
}
```

Response Parameters when data is pushed

| Parameter   | Type    | Description                                                |
|-------------|---------|------------------------------------------------------------|
| arg         | Object  | Successfully subscribed channel                            |
| data        | Array   | Subscribed data                                            |
