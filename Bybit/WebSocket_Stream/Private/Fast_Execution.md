# Fast Execution

Fast execution stream significantly reduces data latency compared to original "execution" stream. However, it pushes limited execution type of trades, and fewer data fields.

**All-In-One Topic:** execution.fast  
**Categorised Topic:** execution.fast.linear, execution.fast.inverse, execution.fast.spot

> **info**  
> Supports all Perps, Futures and Spot execution but does not support Options for now  
> You can only receive `execType=Trade` update

### Response Parameters

| Parameter     | Type   | Comments                                                                                          |
|---------------|--------|-------------------------------------------------------------------------------------------------|
| topic         | string | Topic name                                                                                       |
| creationTime | number | Data created timestamp (ms)                                                                     |
| data          | array  | Object                                                                                          |

Inside `data` array objects:

| Parameter     | Type    | Comments                                                                                        |
|---------------|---------|-------------------------------------------------------------------------------------------------|
| category      | string  | Product type  <br>UTA2.0, UTA1.0: linear, inverse, spot  <br>Classic account: linear, inverse, spot.                                                      |
| symbol        | string  | Symbol name                                                                                      |
| orderId      | string  | Order ID                                                                                        |
| isMaker      | boolean | true: Maker, false: Taker                                                                       |
| orderLinkId  | string  | User customized order ID  <br>Maker trade is always `""`  <br>If a maker order in order book is converted to taker (by price amend), orderLinkId is also `""`             |
| execId       | string  | Execution ID                                                                                   |
| execPrice    | string  | Execution price                                                                                |
| execQty      | string  | Execution quantity                                                                            |
| side         | string  | Side. Buy, Sell                                                                              |
| execTime     | string  | Executed timestamp (ms)                                                                       |
| seq          | long    | Cross sequence, used to associate each fill and each position update. Different symbols may have same seq, use seq + symbol to check unique   |

### Subscribe Example

```json
{
    "op": "subscribe",
    "args": [
        "execution.fast"
    ]
}
```

### Stream Example

```json
{
    "topic": "execution.fast",
    "creationTime": 1716800399338,
    "data": [
        {
            "category": "linear",
            "symbol": "ICPUSDT",
            "execId": "3510f361-0add-5c7b-a2e7-9679810944fc",
            "execPrice": "12.015",
            "execQty": "3000",
            "orderId": "443d63fa-b4c3-4297-b7b1-23bca88b04dc",
            "isMaker": false,
            "orderLinkId": "test-00001",
            "side": "Sell",
            "execTime": "1716800399334",
            "seq": 34771365464
        }
    ]
}
```

***

