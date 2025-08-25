## All Market Tickers Stream

24hr rolling window ticker statistics for all symbols that changed in an array. These are NOT the statistics of the UTC day, but a 24hr rolling window for the previous 24hrs. Note that only tickers that have changed will be present in the array.

**Stream Name:** `!ticker@arr`

**Update Speed:** 1000ms

**Payload:**
```json
[
  {
    // Same as <symbol>@ticker payload
  }
]
```