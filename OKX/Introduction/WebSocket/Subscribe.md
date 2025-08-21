### Subscribe
Subscription Instructions

Request format description

```json
{
  "op": "subscribe",
  "args": ["<SubscriptionTopic>"]
}
WebSocket channels are divided into two categories: public and private channels.
```

Public channels -- No authentication is required, include tickers channel, K-Line channel, limit price channel, order book channel, and mark price channel etc.

Private channels -- including account channel, order channel, and position channel, etc -- require log in.

Users can choose to subscribe to one or more channels, and the total length of multiple channels cannot exceed 64 KB.

Below is an example of subscription parameters. The requirement of subscription parameters for each channel is different. For details please refer to the specification of each channels.

Request Example

```json
{
    "op":"subscribe",
    "args":[
        {
            "channel":"tickers",
            "instId":"BTC-USDT"
        }
    ]
}
Request parameters
```

| Parameter    | Type             | Required | Description                                           |
|--------------|-----------------|----------|-------------------------------------------------------|
| op           | String           | Yes      | Operation type; e.g., "subscribe".                  |
| args         | Array of objects | Yes      | List of channels to subscribe to.                   |
| > channel    | String           | Yes      | Name of the channel.                                 |
| > instType   | String           | No       | Instrument type. Options: SPOT, MARGIN, SWAP, FUTURES, OPTION, ANY. |
| > instFamily | String           | No       | Instrument family; applicable to FUTURES/SWAP/OPTION. |
| > instId     | String           | No       | Specific instrument ID.                              |

Response Example

```json
{
    "event": "subscribe",
    "arg": {
        "channel": "tickers",
        "instId": "BTC-USDT"
    },
    "connId": "accb8e21"
}
Return parameters
```

| Parameter    | Type   | Required | Description                                                                 |
|--------------|--------|----------|-----------------------------------------------------------------------------|
| event        | String | Yes      | Event type; e.g., "subscribe".                                             |
| error        |        | No       | Contains error information if subscription fails.                           |
| arg          | Object | No       | Details of the subscribed channel.                                          |
| > channel    | String | Yes      | Channel name.                                                              |
| > instType   | String | No       | Instrument type. Options: SPOT, MARGIN, SWAP, FUTURES, OPTION, ANY.       |
| > instFamily | String | No       | Instrument family; applicable to FUTURES/SWAP/OPTION.                      |
| > instId     | String | No       | Specific instrument ID.                                                     |
| code         | String | No       | Error code, if any.                                                        |
| msg          | String | No       | Error message, if any.                                                     |
| connId       | String | Yes      | WebSocket connection ID.                                                   |
