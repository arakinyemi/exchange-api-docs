### API key security
 To improve security, we strongly recommend clients linked the API key to IP addresses
Each API key can bind up to 20 IP addresses, which support IPv4/IPv6 and network segment formats.
 API keys that are not linked to an IP address and have `trade` or `withdraw` permissions will expire after 14 days of inactivity. (The API key of demo trading will not expire)
Only when the user calls an API that requires API key authentication will it be considered as the API key is used.
Calling an API that does not require API key authentication will not be considered used even if API key information is passed in.
For websocket, only operation of logging in will be considered to have used the API key. Any operation though the connection after logging in (such as subscribing/placing an order) will not be considered to have used the API key. Please pay attention.
Users can get the usage records of the API key with trade or withdraw permissions but unlinked to any IP address though Security Center.
