## Individual Symbol Rolling Window Statistics Streams

Rolling window ticker statistics for a single symbol, computed over multiple windows.

**Stream Name:** `<symbol>@ticker_<window_size>`

**Window Sizes:** 1h,4h,1d

**Update Speed:** 1000ms

**Note:** This stream is different from the `<symbol>@ticker` stream. The open time "O" always starts on a minute, while the closing time "C" is the current time of the update. As such, the effective window might be up to 59999ms wider than `<window_size>`.

**Payload:**
```json
{
  "e": "1hTicker",    // Event type
  "E": 1672515782136, // Event time
  "s": "BNBBTC",      // Symbol
  "p": "0.0015",      // Price change
  "P": "250.00",      // Price change percent
  "o": "0.0010",      // Open price
  "h": "0.0025",      // High price
  "l": "0.0010",      // Low price
  "c": "0.0025",      // Last price
  "w": "0.0018",      // Weighted average price
  "v": "10000",       // Total traded base asset volume
  "q": "18",          // Total traded quote asset volume
  "O": 0,             // Statistics open time
  "C": 1675216573749, // Statistics close time
  "F": 0,             // First trade ID
  "L": 18150,         // Last trade Id
  "n": 18151          // Total number of trades
}
```