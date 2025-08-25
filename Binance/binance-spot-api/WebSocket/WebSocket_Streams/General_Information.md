## General WSS information

The base endpoint is: `wss://stream.binance.com:9443` or `wss://stream.binance.com:443`.

Streams can be accessed either in a single raw stream or in a combined stream.
- Raw streams are accessed at `/ws/<streamName>`
- Combined streams are accessed at `/stream?streams=<streamName1>/<streamName2>/<streamName3>`

Combined stream events are wrapped as follows: `{"stream":"<streamName>","data":<rawPayload>}`

All symbols for streams are lowercase

A single connection to stream.binance.com is only valid for 24 hours; expect to be disconnected at the 24 hour mark

The WebSocket server will send a ping frame every 20 seconds.

If the WebSocket server does not receive a pong frame back from the connection within a minute the connection will be disconnected.

When you receive a ping, you must send a pong with a copy of ping's payload as soon as possible.

Unsolicited pong frames are allowed, but will not prevent disconnection. It is recommended that the payload for these pong frames are empty.

The base endpoint `wss://data-stream.binance.vision` can be subscribed to receive only market data messages.

User data stream is NOT available from this URL.

All time and timestamp related fields are milliseconds by default. To receive the information in microseconds, please add the parameter `timeUnit=MICROSECOND` or `timeUnit=microsecond` in the URL.

For example: `/stream?streams=btcusdt@trade&timeUnit=MICROSECOND`




























