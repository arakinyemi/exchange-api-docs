## Individual Symbol Mini Ticker Stream

24hr rolling window mini-ticker statistics. These are NOT the statistics of the UTC day, but a 24hr rolling window for the previous 24hrs.

**Stream Name:** `<symbol>@miniTicker`

**Update Speed:** 1000ms

**Payload:**
```json
{
  "e": "24hrMiniTicker",  // Event type
  "E": 1672515782136,     // Event time
  "s": "BNBBTC",          // Symbol
  "c": "0.0025",          // Close price
  "o": "0.0010",          // Open price
  "h": "0.0025",          // High price
  "l": "0.0010",          // Low price
  "v": "10000",           // Total traded base asset volume
  "q": "18"               // Total traded quote asset volume
}
```