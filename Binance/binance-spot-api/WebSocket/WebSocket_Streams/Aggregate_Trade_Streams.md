## Aggregate Trade Streams

The Aggregate Trade Streams push trade information that is aggregated for a single taker order.

**Stream Name:** `<symbol>@aggTrade`

**Update Speed:** Real-time

**Payload:**
```json
{
  "e": "aggTrade",    // Event type
  "E": 1672515782136, // Event time
  "s": "BNBBTC",      // Symbol
  "a": 12345,         // Aggregate trade ID
  "p": "0.001",       // Price
  "q": "100",         // Quantity
  "f": 100,           // First trade ID
  "l": 105,           // Last trade ID
  "T": 1672515782136, // Trade time
  "m": true,          // Is the buyer the market maker?
  "M": true           // Ignore
}
```