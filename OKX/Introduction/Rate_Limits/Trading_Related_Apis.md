### Trading-related APIs
For Trading-related APIs (place order, cancel order, and amend order) the following conditions apply:

Rate limits are shared across the REST and WebSocket channels.

Rate limits for placing orders, amending orders, and cancelling orders are independent from each other.

Rate limits are defined on the Instrument ID level (except Options)

Rate limits for Options are defined based on the Instrument Family level. Refer to the Get instruments endpoint to view Instrument Family information.

Rate limits for a multiple order endpoint and a single order endpoint are also independent, with the exception being when there is only one order sent to a multiple order endpoint, the order will be counted as a single order and adopt the single order rate limit.
