# Websocket Trade Guideline

### URL

- **Mainnet:**  
  `wss://stream.bybit.com/v5/trade`  
  *Info:* Turkey users from "www.bybit-tr.com", use `wss://stream.bybit-tr.com/v5/trade`  
  Kazakhstan users from "www.bybit.kz", use `wss://stream.bybit.kz/v5/trade`  

- **Testnet:**  
  `wss://stream-testnet.bybit.com/v5/trade`

### Support
- UTA2.0: category=linear, spot, option, inverse  
- UTA1.0: category=linear, spot, option



## Authentication

### Request Parameters

| Parameter    | Required | Type    | Comments                  |
|--------------|----------|---------|---------------------------|
| reqId        | false    | string  | Optional field to match response. Not returned if not passed |
| op           | true     | string  | Op type: `auth`           |
| args         | true     | string  | `["api key", expiry timestamp, "signature"]` |

### Response Parameters

| Parameter    | Type    | Comments                                                                     |
|--------------|---------|-------------------------------------------------------------------------------|
| reqId        | string  | Returned if passed in request                                                |
| retCode      | integer | 0: auth success; 20001: repeat auth; 10004: invalid sign; 10001: param error   |
| retMsg       | string  | OK or error message                                                          |
| op           | string  | Op type                                                                      |
| connId       | string  | Connection id                                                                |

### Request Example

```json
{
    "op": "auth",
    "args": [
        "XXXXXX",
        1711010121452,
        "ec71040eff72b163a36153d770b69d6637bcb29348fbfbb16c269a76595ececf"
    ]
}
```

### Response Example

```json
{
    "retCode": 0,
    "retMsg": "OK",
    "op": "auth",
    "connId": "cnt5leec0hvan15eukcg-2t"
}
```

***

## Create/Amend/Cancel Order

### Request Parameters

| Parameter      | Required | Type    | Comments                                                                                                  |
|----------------|----------|---------|-----------------------------------------------------------------------------------------------------------|
| reqId          | false    | string  | Uniqueness id (max length: 36). Duplicates cause "20006" error                                            |
| header         | true     | object  | Request headers                                                                                           |
| &gt; X-BAPI-TIMESTAMP | true | string  | Current timestamp                                                                                          |
| &gt; X-BAPI-RECV-WINDOW | false | string  | 5000 ms default; request rejected if outside: Bybit_server_time - window <= timestamp < Bybit_server_time + 1000 |
| &gt; Referer     | false    | string  | Referer identifier for API broker user                                                                   |
| op             | true     | string  | Op type: `order.create`, `order.amend`, `order.cancel`                                                   |
| args           | true     | array   | Args array, supports one item only for now                                                               |

### Response Parameters

| Parameter         | Type    | Comments                                                                                                     |
|-------------------|---------|--------------------------------------------------------------------------------------------------------------|
| reqId             | string  | Returned if passed                                                                                            |
| retCode           | integer | 0: success; errors such as 10403 (rate limit), 10404 (invalid op or category), 10429 (system frequency protection), 20006 (duplicate reqId), 10016, 10019  |
| retMsg            | string  | OK or error message                                                                                          |
| op                | string  | Op type                                                                                                      |
| data              | object  | Business data, same as REST API response                                                                     |
| retExtInfo        | object  | Always empty                                                                                                |
| header            | object  | Header info                                                                                                |
| &gt; TraceId       | string  | Trace ID used for request tracking                                                                           |
| &gt; Timenow       | string  | Current timestamp                                                                                            |
| &gt; X-Bapi-Limit  | string  | Total rate limit for this op type                                                                             |
| &gt; X-Bapi-Limit-Status | string | Remaining rate limit                                                                                       |
| &gt; X-Bapi-Limit-Reset-Timestamp | string | Timestamp when limit resets if exceeded                                                           |
| connId            | string  | Connection id                                                                                                |

> **info**  
> Ack of create/amend/cancel order indicates request accepted successfully. Use websocket order stream to confirm order status.

### Request Example

```json
{
    "reqId": "test-005",
    "header": {
        "X-BAPI-TIMESTAMP": "1711001595207",
        "X-BAPI-RECV-WINDOW": "8000",
        "Referer": "bot-001"  // for api broker
    },
    "op": "order.create",
    "args": [
        {
            "symbol": "ETHUSDT",
            "side": "Buy",
            "orderType": "Limit",
            "qty": "0.2",
            "price": "2800",
            "category": "linear",
            "timeInForce": "PostOnly"
        }
    ]
}
```

### Response Example

```json
{
    "reqId": "test-005",
    "retCode": 0,
    "retMsg": "OK",
    "op": "order.create",
    "data": {
        "orderId": "a4c1718e-fe53-4659-a118-1f6ecce04ad9",
        "orderLinkId": ""
    },
    "retExtInfo": {},
    "header": {
        "X-Bapi-Limit": "10",
        "X-Bapi-Limit-Status": "9",
        "X-Bapi-Limit-Reset-Timestamp": "1711001595208",
        "Traceid": "38b7977b430f9bd228f4b19724794dfd",
        "Timenow": "1711001595209"
    },
    "connId": "cnt5leec0hvan15eukcg-2v"
}
```

***

## Batch Create/Amend/Cancel Order

> **info**  
> Max 20 orders (option, inverse, linear), 10 orders (spot) per request.  
> Response divided into success/error list and created order details list.  
> Option rate limit counts actual request sent; e.g., 10 reqs/sec limit means 20 * 10 = 200 orders per second possible.  
> Account rate limit shared between websocket and HTTP batch orders.  
> Ack indicates request accepted successfully; use websocket to confirm order status.

### Request Parameters

| Parameter      | Required | Type    | Comments                                                                                                  |
|----------------|----------|---------|-----------------------------------------------------------------------------------------------------------|
| reqId          | false    | string  | Uniqueness id (max length 36), duplicates yield "20006" error                                            |
| header         | true     | object  | Request headers                                                                                           |
| &gt; X-BAPI-TIMESTAMP | true | string  | Current timestamp                                                                                          |
| &gt; X-BAPI-RECV-WINDOW | false | string  | 5000 ms default; request rejected if timestamp invalid                                                  |
| &gt; Referer    | false    | string  | Referer for API broker users                                                                              |
| op             | true     | string  | `order.create-batch`, `order.amend-batch`, `order.cancel-batch`                                           |
| args           | true     | array   | Args array                                                                                               |

### Response Parameters

| Parameter         | Type    | Comments                                                                                                     |
|-------------------|---------|--------------------------------------------------------------------------------------------------------------|
| reqId             | string  | Returned if passed                                                                                            |
| retCode           | integer | 0: success; errors like 10403 (rate limit), 10404 (invalid op/category), 10429 (frequency protection), 20006 (dup reqId), 10016, 10019                  |
| retMsg            | string  | OK or error message                                                                                          |
| op                | string  | Op type                                                                                                      |
| data              | object  | Business data, same as REST API                                                                               |
| retExtInfo        | object  |  
> list: array of `{code: number, msg: string}` for batch order errors                                               |
| header            | object  | Header info                                                                                                  |
| &gt; TraceId       | string  | Trace ID                                                                                                     |
| &gt; Timenow       | string  | Current timestamp                                                                                            |
| &gt; X-Bapi-Limit  | string  | Total rate limit                                                                                            |
| &gt; X-Bapi-Limit-Status | string | Remaining rate limit                                                                                      |
| &gt; X-Bapi-Limit-Reset-Timestamp | string | Timestamp of next reset                                                                              |
| connId            | string  | Connection id                                                                                                |

### Request Example

```json
{
    "op": "order.create-batch",
    "header": {
        "X-BAPI-TIMESTAMP": "1740453381256",
        "X-BAPI-RECV-WINDOW": "1000"
    },
    "args": [
        {
            "category": "linear",
            "request": [
                {
                    "symbol": "SOLUSDT",
                    "qty": "10",
                    "price": "500",
                    "orderType": "Limit",
                    "timeInForce": "GTC",
                    "orderLinkId": "-batch-000",
                    "side": "Buy"
                },
                {
                    "symbol": "SOLUSDT",
                    "qty": "20",
                    "price": "1000",
                    "orderType": "Limit",
                    "timeInForce": "GTC",
                    "orderLinkId": "batch-001",
                    "side": "Buy"
                },
                {
                    "symbol": "SOLUSDT",
                    "qty": "30",
                    "price": "1500",
                    "orderType": "Limit",
                    "timeInForce": "GTC",
                    "orderLinkId": "batch-002",
                    "side": "Buy"
                }
            ]
        }
    ]
}
```

### Response Example

```json
{
    "retCode": 0,
    "retMsg": "OK",
    "op": "order.create-batch",
    "data": {
        "list": [
            {
                "category": "linear",
                "symbol": "SOLUSDT",
                "orderId": "",
                "orderLinkId": "batch-000",
                "createAt": ""
            },
            {
                "category": "linear",
                "symbol": "SOLUSDT",
                "orderId": "",
                "orderLinkId": "batch-001",
                "createAt": ""
            },
            {
                "category": "linear",
                "symbol": "SOLUSDT",
                "orderId": "",
                "orderLinkId": "batch-002",
                "createAt": ""
            }
        ]
    },
    "retExtInfo": {
        "list": [
            {
                "code": 10001,
                "msg": "position idx not match position mode"
            },
            {
                "code": 10001,
                "msg": "position idx not match position mode"
            },
            {
                "code": 10001,
                "msg": "position idx not match position mode"
            }
        ]
    },
    "header": {
        "Timenow": "1740453408556",
        "X-Bapi-Limit": "150",
        "X-Bapi-Limit-Status": "147",
        "X-Bapi-Limit-Reset-Timestamp": "1740453408555",
        "Traceid": "0e32b551b3e17aae77651aadf6a5be80"
    },
    "connId": "cupviqn88smf24t2kpb0-536o"
}
```

***

## Ping

### Request Parameters

| Parameter | Required | Type   | Comments   |
|-----------|----------|--------|------------|
| op        | true     | string | Op type: `ping` |

### Response Parameters

| Parameter | Type    | Comments      |
|-----------|---------|---------------|
| retCode   | integer | Result code   |
| retMsg    | string  | Result message|
| op        | string  | Op type `pong`|
| data      | array   | One item: current timestamp (string) |
| connId    | string  | Connection id |

### Request Example

```json
{
    "op": "ping"
}
```

### Response Example

```json
{
    "retCode": 0,
    "retMsg": "OK",
    "op": "pong",
    "data": [
        "1711002002529"
    ],
    "connId": "cnt5leec0hvan15eukcg-2v"
}
```


