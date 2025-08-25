## Trade Streams

The Trade Streams push raw trade information; each trade has a unique buyer and seller.

**Stream Name:** `<symbol>@trade`

**Update Speed:** Real-time

**Payload:**
```json
{
  "e": "trade",       // Event type
  "E": 1672515782136, // Event time
  "s": "BNBBTC",      // Symbol
  "t": 12345,         // Trade ID
  "p": "0.001",       // Price
  "q": "100",         // Quantity
  "T": 1672515782136, // Trade time
  "m": true,          // Is the buyer the market maker?
  "M": true           // Ignore
}
```