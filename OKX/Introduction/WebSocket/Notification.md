### Notification
WebSocket has introduced a new message type (event = notice).


Client will receive the information in the following scenarios:

Websocket disconnect for service upgrade
60 seconds prior to the upgrade of the WebSocket service, the notification message will be sent to users indicating that the connection will soon be disconnected. Users are encouraged to establish a new connection to prevent any disruptions caused by disconnection.

Response Example

```json
{
    "event": "notice",
    "code": "64008",
    "msg": "The connection will soon be closed for a service upgrade. Please reconnect.",
    "connId": "a4d3ae55"
}
```


The feature is supported by WebSocket Public (/ws/v5/public) and Private (/ws/v5/private) for now.
