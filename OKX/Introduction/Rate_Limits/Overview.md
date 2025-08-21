### Overview


Our REST and WebSocket APIs use rate limits to protect our APIs against malicious usage so our trading platform can operate reliably and fairly.
When a request is rejected by our system due to rate limits, the system returns error code 50011 (Rate limit reached. Please refer to API documentation and throttle requests accordingly).
The rate limit is different for each endpoint. You can find the limit for each endpoint from the endpoint details. Rate limit definitions are detailed below:

WebSocket login and subscription rate limits are based on connection.

Public unauthenticated REST rate limits are based on IP address.

Private REST rate limits are based on User ID (sub-accounts have individual User IDs).

WebSocket order management rate limits are based on User ID (sub-accounts have individual User IDs).
