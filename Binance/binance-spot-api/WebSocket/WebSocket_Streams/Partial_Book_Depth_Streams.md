## Partial Book Depth Streams

Top `<levels>` bids and asks, pushed every second. Valid `<levels>` are 5, 10, or 20.

**Stream Names:** `<symbol>@depth<levels>` OR `<symbol>@depth<levels>@100ms`

**Update Speed:** 1000ms or 100ms

**Payload:**
```json
{
  "lastUpdateId": 160,  // Last update ID
  "bids": [             // Bids to be updated
    [
      "0.0024",         // Price level to be updated
      "10"              // Quantity
    ]
  ],
  "asks": [             // Asks to be updated
    [
      "0.0026",         // Price level to be updated
      "100"             // Quantity
    ]
  ]
}
```
