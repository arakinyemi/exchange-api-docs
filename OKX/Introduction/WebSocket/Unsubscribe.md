### Unsubscribe
Unsubscribe from one or more channels.

Request format description

```json
{
  "op": "unsubscribe",
  "args": ["< SubscriptionTopic> "]
}
```

Request Example

```json
{
  "op": "unsubscribe",
  "args": [
    {
      "channel": "tickers",
      "instId": "BTC-USDT"
    }
  ]
}
```

Request parameters

| Parameter    | Type             | Required | Description                                           |
|--------------|-----------------|----------|-------------------------------------------------------|
| op           | String           | Yes      | Operation type; e.g., "unsubscribe".                |
| args         | Array of objects | Yes      | List of channels to unsubscribe from.               |
| > channel    | String           | Yes      | Name of the channel.                                 |
| > instType   | String           | No       | Instrument type. Options: SPOT, MARGIN, SWAP, FUTURES, OPTION, ANY. |
| > instFamily | String           | No       | Instrument family; applicable to FUTURES/SWAP/OPTION. |
| > instId     | String           | No       | Specific instrument ID.                              |

Response Example

```json
{
    "event": "unsubscribe",
    "arg": {
        "channel": "tickers",
        "instId": "BTC-USDT"
    },
    "connId": "d0b44253"
}
Response parameters
```

| Parameter    | Type   | Required | Description                                                                 |
|--------------|--------|----------|-----------------------------------------------------------------------------|
| event        | String | Yes      | Event type; e.g., "unsubscribe".                                           |
| error        |        | No       | Contains error information if unsubscribe fails.                            |
| arg          | Object | No       | Details of the unsubscribed channel.                                        |
| > channel    | String | Yes      | Channel name.                                                              |
| > instType   | String | No       | Instrument type. Options: SPOT, MARGIN, SWAP, FUTURES, OPTION.             |
| > instFamily | String | No       | Instrument family; applicable to FUTURES/SWAP/OPTION.                      |
| > instId     | String | No       | Specific instrument ID.                                                     |
| code         | String | No       | Error code, if any.                                                        |
| msg          | String | No       | Error message, if any.                                                     |
