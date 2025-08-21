### Connection count limit
The limit will be set at 30 WebSocket connections per specific WebSocket channel per sub-account. Each WebSocket connection is identified by the unique connId.



The WebSocket channels subject to this limitation are as follows:

Orders channel
Account channel
Positions channel
Balance and positions channel
Position risk warning channel
Account greeks channel
If users subscribe to the same channel through the same WebSocket connection through multiple arguments, for example, by using {"channel": "orders", "instType": "ANY"} and {"channel": "orders", "instType": "SWAP"}, it will be counted once only. If users subscribe to the listed channels (such as orders and accounts) using either the same or different connections, it will not affect the counting, as these are considered as two different channels. The system calculates the number of WebSocket connections per channel.



The platform will send the number of active connections to clients through the channel-conn-count event message to new channel subscriptions.



When the limit is breached, generally the latest connection that sends the subscription request will be rejected. Client will receive the usual subscription acknowledgement, followed by the channel-conn-count-error from the connection that the subscription has been terminated. In exceptional circumstances the platform may unsubscribe existing connections.


Order operations through WebSocket, including place, amend and cancel orders, are not impacted through this change.


Connection count update

```json
{
    "event":"channel-conn-count",
    "channel":"orders",
    "connCount": "2",
    "connId":"abcd1234"
}
```

Connection limit error

```json
{
    "event": "channel-conn-count-error",
    "channel": "orders",
    "connCount": "20",
    "connId":"a4d3ae55"
}
```
