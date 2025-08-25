## All Market Rolling Window Statistics Streams

Rolling window ticker statistics for all market symbols, computed over multiple windows. Note that only tickers that have changed will be present in the array.

**Stream Name:** `!ticker_<window-size>@arr`

**Window Size:** 1h,4h,1d

**Update Speed:** 1000ms

**Payload:**
```json
[
  {
    // Same as <symbol>@ticker_<window_size> payload,
    // one for each symbol updated within the interval.
  }
]
```