## Average Price

Average price streams push changes in the average price over a fixed time interval.

**Stream Name:** `<symbol>@avgPrice`

**Update Speed:** 1000ms

**Payload:**
```json
{
  "e": "avgPrice",          // Event type
  "E": 1693907033000,       // Event time
  "s": "BTCUSDT",           // Symbol
  "i": "5m",                // Average price interval
  "w": "25776.86000000",    // Average price
  "T": 1693907032213        // Last trade time
}
```