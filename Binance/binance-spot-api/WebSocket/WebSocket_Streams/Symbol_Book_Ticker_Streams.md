## Individual Symbol Book Ticker Streams

Pushes any update to the best bid or ask's price or quantity in real-time for a specified symbol. Multiple `<symbol>@bookTicker` streams can be subscribed to over one connection.

**Stream Name:** `<symbol>@bookTicker`

**Update Speed:** Real-time

**Payload:**
```json
{
  "u":400900217,     // order book updateId
  "s":"BNBUSDT",     // symbol
  "b":"25.35190000", // best bid price
  "B":"31.21000000", // best bid qty
  "a":"25.36520000", // best ask price
  "A":"40.66000000"  // best ask qty
}
```