## How to manage a local order book correctly

1. Open a WebSocket connection to `wss://stream.binance.com:9443/ws/bnbbtc@depth`.
2. Buffer the events received from the stream. Note the `U` of the first event you received.
3. Get a depth snapshot from `https://api.binance.com/api/v3/depth?symbol=BNBBTC&limit=5000`.
4. If the `lastUpdateId` from the snapshot is strictly less than the `U` from step 2, go back to step 3.
5. In the buffered events, discard any event where `u` is <= `lastUpdateId` of the snapshot. The first buffered event should now have `lastUpdateId` within its `[U;u]` range.
6. Set your local order book to the snapshot. Its update ID is `lastUpdateId`.
7. Apply the update procedure below to all buffered events, and then to all subsequent events received.

To apply an event to your local order book, follow this update procedure:

1. If the event `u` (last update ID) is < the update ID of your local order book, ignore the event.
2. If the event `U` (first update ID) is > the update ID of your local order book, something went wrong. Discard your local order book and restart the process from the beginning.
3. For each price level in bids (`b`) and asks (`a`), set the new quantity in the order book:
   - If the price level does not exist in the order book, insert it with new quantity.
   - If the quantity is zero, remove the price level from the order book.
4. Set the order book update ID to the last update ID (`u`) in the processed event.

> **[!NOTE]** Since depth snapshots retrieved from the API have a limit on the number of price levels (5000 on each side maximum), you won't learn the quantities for the levels outside of the initial snapshot unless they change.
>
> So be careful when using the information for those levels, since they might not reflect the full view of the order book.
>
> However, for most use cases, seeing 5000 levels on each side is enough to understand the market and trade effectively.