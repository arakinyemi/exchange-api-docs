## WebSocket Limits

WebSocket connections have a limit of 5 incoming messages per second. A message is considered:
- A PING frame
- A PONG frame  
- A JSON controlled message (e.g. subscribe, unsubscribe)

A connection that goes beyond the limit will be disconnected; IPs that are repeatedly disconnected may be banned.

A single connection can listen to a maximum of 1024 streams.

There is a limit of 300 connections per attempt every 5 minutes per IP.
