### Overview


The API endpoints of Market Data do not require authentication.  
There are multiple services for market data, and each service has an independent cache. A random service will be requested for every request. So for two requests, it’s expected that the data obtained in the second request is earlier than the first request.
