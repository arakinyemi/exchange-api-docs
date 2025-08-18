**Overview**
Welcome to our API documentation. OKX provides REST and WebSocket APIs to suit your trading needs.


**Generating an API key**
Create an API key on the website before signing any requests. After creating an API key, keep the following information safe:

API key
Secret key
Passphrase


The system returns randomly-generated API keys and SecretKeys. You will need to provide the Passphrase to access the API. We store the salted hash of your Passphrase for authentication. We cannot recover the Passphrase if you have lost it. You will need to create a new set of API key.


**API key permissions**
There are three permissions below that can be associated with an API key. One or more permission can be assigned to any key.

Read : Can request and view account info such as bills and order history which need read permission
Trade : Can place and cancel orders, funding transfer, make settings which need write permission
Withdraw : Can make withdrawals


**API key security**
 To improve security, we strongly recommend clients linked the API key to IP addresses
Each API key can bind up to 20 IP addresses, which support IPv4/IPv6 and network segment formats.
 API keys that are not linked to an IP address and have `trade` or `withdraw` permissions will expire after 14 days of inactivity. (The API key of demo trading will not expire)
Only when the user calls an API that requires API key authentication will it be considered as the API key is used.
Calling an API that does not require API key authentication will not be considered used even if API key information is passed in.
For websocket, only operation of logging in will be considered to have used the API key. Any operation though the connection after logging in (such as subscribing/placing an order) will not be considered to have used the API key. Please pay attention.
Users can get the usage records of the API key with trade or withdraw permissions but unlinked to any IP address though Security Center.

**REST Authentication**
Making Requests
All private REST requests must contain the following headers:

OK-ACCESS-KEY The API key as a String.

OK-ACCESS-SIGN The Base64-encoded signature (see Signing Messages subsection for details).

OK-ACCESS-TIMESTAMP The UTC timestamp of your request .e.g : 2020-12-08T09:08:57.715Z

OK-ACCESS-PASSPHRASE The passphrase you specified when creating the API key.

Request bodies should have content type application/json and be in valid JSON format.

Signature
The OK-ACCESS-SIGN header is generated as follows:

Create a pre-hash string of timestamp + method + requestPath + body (where + represents String concatenation).
Prepare the SecretKey.
Sign the pre-hash string with the SecretKey using the HMAC SHA256.
Encode the signature in the Base64 format.
Example: sign=CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(timestamp + 'GET' + '/api/v5/account/balance?ccy=BTC', SecretKey))

The timestamp value is the same as the OK-ACCESS-TIMESTAMP header with millisecond ISO format, e.g. 2020-12-08T09:08:57.715Z.

The request method should be in UPPERCASE: e.g. GET and POST.

The requestPath is the path of requesting an endpoint.

Example: /api/v5/account/balance

The body refers to the String of the request body. It can be omitted if there is no request body (frequently the case for GET requests).

Example: {"instId":"BTC-USDT","lever":"5","mgnMode":"isolated"}

 `GET` request parameters are counted as requestpath, not body
The SecretKey is generated when you create an API key.

Example: 22582BD0CFF14C41EDBF1AB98506286D


**WebSocket**
Overview
WebSocket is a new HTML5 protocol that achieves full-duplex data transmission between the client and server, allowing data to be transferred effectively in both directions. A connection between the client and server can be established with just one handshake. The server will then be able to push data to the client according to preset rules. Its advantages include:

The WebSocket request header size for data transmission between client and server is only 2 bytes.
Either the client or server can initiate data transmission.
There's no need to repeatedly create and delete TCP connections, saving resources on bandwidth and server.
 We recommend developers use WebSocket API to retrieve market data and order book depth.
Connect
Connection limit: 3 requests per second (based on IP)

When subscribing to a public channel, use the address of the public service. When subscribing to a private channel, use the address of the private service

Request limit:

The total number of 'subscribe'/'unsubscribe'/'login' requests per connection is limited to 480 times per hour.

If there’s a network problem, the system will automatically disable the connection.

The connection will break automatically if the subscription is not established or data has not been pushed for more than 30 seconds.

To keep the connection stable:

1. Set a timer of N seconds whenever a response message is received, where N is less than 30.

2. If the timer is triggered, which means that no new message is received within N seconds, send the String 'ping'.

3. Expect a 'pong' as a response. If the response message is not received within N seconds, please raise an error or reconnect.

Connection count limit
The limit will be set at 30 WebSocket connections per specific WebSocket channel per sub-account. Each WebSocket connection is identified by the unique connId.



The WebSocket channels subject to this limitation are as follows:

Orders channel
Account channel
Positions channel
Balance and positions channel
Position risk warning channel
Account greeks channel
If users subscribe to the same channel through the same WebSocket connection through multiple arguments, for example, by using {"channel": "orders", "instType": "ANY"} and {"channel": "orders", "instType": "SWAP"}, it will be counted once only. If users subscribe to the listed channels (such as orders and accounts) using either the same or different connections, it will not affect the counting, as these are considered as two different channels. The system calculates the number of WebSocket connections per channel.



The platform will send the number of active connections to clients through the channel-conn-count event message to new channel subscriptions.



When the limit is breached, generally the latest connection that sends the subscription request will be rejected. Client will receive the usual subscription acknowledgement, followed by the channel-conn-count-error from the connection that the subscription has been terminated. In exceptional circumstances the platform may unsubscribe existing connections.


Order operations through WebSocket, including place, amend and cancel orders, are not impacted through this change.


Connection count update

{
    "event":"channel-conn-count",
    "channel":"orders",
    "connCount": "2",
    "connId":"abcd1234"
}

Connection limit error

{
    "event": "channel-conn-count-error",
    "channel": "orders",
    "connCount": "20",
    "connId":"a4d3ae55"
}


Login
Request Example

{
  "op": "login",
  "args": [
    {
      "apiKey": "******",
      "passphrase": "******",
      "timestamp": "1538054050",
      "sign": "7L+zFQ+CEgGu5rzCj4+BdV2/uUHGqddA9pI6ztsRRPs="
    }
  ]
}
Request Parameters
| Parameter | Type              | Required | Description                                                                 |
|-----------|-------------------|----------|-----------------------------------------------------------------------------|
| op        | String            | Yes      | Operation type; for login, use `"login"`.                                   |
| args      | Array of objects  | Yes      | List of accounts to log in.                                                  |
| > apiKey  | String            | Yes      | Your API Key.                                                               |
| > passphrase | String         | Yes      | The passphrase you set when creating the API key.                            |
| > timestamp | String          | Yes      | Unix Epoch time in seconds.                                                  |
| > sign    | String            | Yes      | Signature string generated using HMAC SHA256 with your SecretKey.             |

Successful Response Example

{
  "event": "login",
  "code": "0",
  "msg": "",
  "connId": "a4d3ae55"
}
Failure Response Example

{
  "event": "error",
  "code": "60009",
  "msg": "Login failed.",
  "connId": "a4d3ae55"
}

Response parameters
| Parameter | Type   | Required | Description                                                                 |
|-----------|--------|----------|-----------------------------------------------------------------------------|
| event     | String | Yes      | Operation type; e.g., "login".                                             |
| error     |        | No       | Contains error information if login fails.                                  |
| code      | String | No       | Error code, if any.                                                         |
| msg       | String | No       | Error message, if any.                                                      |
| connId    | String | Yes      | WebSocket connection ID.                                                    |
| apiKey    |        | Yes      | Unique identification for invoking API. Requires user to apply one manually.|

passphrase: API Key password

timestamp: the Unix Epoch time, the unit is seconds, e.g. 1704876947

sign: signature string, the signature algorithm is as follows:

First concatenate timestamp, method, requestPath, strings, then use HMAC SHA256 method to encrypt the concatenated string with SecretKey, and then perform Base64 encoding.

secretKey: The security key generated when the user applies for API key, e.g. 22582BD0CFF14C41EDBF1AB98506286D

Example of timestamp: const timestamp = '' + Date.now() / 1,000

Among sign example: sign=CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(timestamp +'GET'+'/users/self/verify', secretKey))

method: always 'GET'.

requestPath : always '/users/self/verify'

 The request will expire 30 seconds after the timestamp. If your server time differs from the API server time, we recommended using the REST API to query the API server time and then set the timestamp.


Subscribe
Subscription Instructions

Request format description

{
  "op": "subscribe",
  "args": ["<SubscriptionTopic>"]
}
WebSocket channels are divided into two categories: public and private channels.

Public channels -- No authentication is required, include tickers channel, K-Line channel, limit price channel, order book channel, and mark price channel etc.

Private channels -- including account channel, order channel, and position channel, etc -- require log in.

Users can choose to subscribe to one or more channels, and the total length of multiple channels cannot exceed 64 KB.

Below is an example of subscription parameters. The requirement of subscription parameters for each channel is different. For details please refer to the specification of each channels.

Request Example

{
    "op":"subscribe",
    "args":[
        {
            "channel":"tickers",
            "instId":"BTC-USDT"
        }
    ]
}
Request parameters

| Parameter    | Type             | Required | Description                                           |
|--------------|-----------------|----------|-------------------------------------------------------|
| op           | String           | Yes      | Operation type; e.g., "subscribe".                  |
| args         | Array of objects | Yes      | List of channels to subscribe to.                   |
| > channel    | String           | Yes      | Name of the channel.                                 |
| > instType   | String           | No       | Instrument type. Options: SPOT, MARGIN, SWAP, FUTURES, OPTION, ANY. |
| > instFamily | String           | No       | Instrument family; applicable to FUTURES/SWAP/OPTION. |
| > instId     | String           | No       | Specific instrument ID.                              |

Response Example

{
    "event": "subscribe",
    "arg": {
        "channel": "tickers",
        "instId": "BTC-USDT"
    },
    "connId": "accb8e21"
}
Return parameters

| Parameter    | Type   | Required | Description                                                                 |
|--------------|--------|----------|-----------------------------------------------------------------------------|
| event        | String | Yes      | Event type; e.g., "subscribe".                                             |
| error        |        | No       | Contains error information if subscription fails.                           |
| arg          | Object | No       | Details of the subscribed channel.                                          |
| > channel    | String | Yes      | Channel name.                                                              |
| > instType   | String | No       | Instrument type. Options: SPOT, MARGIN, SWAP, FUTURES, OPTION, ANY.       |
| > instFamily | String | No       | Instrument family; applicable to FUTURES/SWAP/OPTION.                      |
| > instId     | String | No       | Specific instrument ID.                                                     |
| code         | String | No       | Error code, if any.                                                        |
| msg          | String | No       | Error message, if any.                                                     |
| connId       | String | Yes      | WebSocket connection ID.                                                   |

Unsubscribe
Unsubscribe from one or more channels.

Request format description

{
  "op": "unsubscribe",
  "args": ["< SubscriptionTopic> "]
}
Request Example

{
  "op": "unsubscribe",
  "args": [
    {
      "channel": "tickers",
      "instId": "BTC-USDT"
    }
  ]
}
Request parameters

| Parameter    | Type             | Required | Description                                           |
|--------------|-----------------|----------|-------------------------------------------------------|
| op           | String           | Yes      | Operation type; e.g., "unsubscribe".                |
| args         | Array of objects | Yes      | List of channels to unsubscribe from.               |
| > channel    | String           | Yes      | Name of the channel.                                 |
| > instType   | String           | No       | Instrument type. Options: SPOT, MARGIN, SWAP, FUTURES, OPTION, ANY. |
| > instFamily | String           | No       | Instrument family; applicable to FUTURES/SWAP/OPTION. |
| > instId     | String           | No       | Specific instrument ID.                              |

Response Example

{
    "event": "unsubscribe",
    "arg": {
        "channel": "tickers",
        "instId": "BTC-USDT"
    },
    "connId": "d0b44253"
}
Response parameters

| Parameter    | Type   | Required | Description                                                                 |
|--------------|--------|----------|-----------------------------------------------------------------------------|
| event        | String | Yes      | Event type; e.g., "unsubscribe".                                           |
| error        |        | No       | Contains error information if unsubscribe fails.                            |
| arg          | Object | No       | Details of the unsubscribed channel.                                        |
| > channel    | String | Yes      | Channel name.                                                              |
| > instType   | String | No       | Instrument type. Options: SPOT, MARGIN, SWAP, FUTURES, OPTION.             |
| > instFamily | String | No       | Instrument family; applicable to FUTURES/SWAP/OPTION.                      |
| > instId     | String | No       | Specific instrument ID.                                                     |
| code         | String | No       | Error code, if any.                                                        |
| msg          | String | No       | Error message, if any.                                                     |

Notification
WebSocket has introduced a new message type (event = notice).


Client will receive the information in the following scenarios:

Websocket disconnect for service upgrade
60 seconds prior to the upgrade of the WebSocket service, the notification message will be sent to users indicating that the connection will soon be disconnected. Users are encouraged to establish a new connection to prevent any disruptions caused by disconnection.

Response Example

{
    "event": "notice",
    "code": "64008",
    "msg": "The connection will soon be closed for a service upgrade. Please reconnect.",
    "connId": "a4d3ae55"
}


The feature is supported by WebSocket Public (/ws/v5/public) and Private (/ws/v5/private) for now.

Account mode
To facilitate your trading experience, please set the appropriate account mode before starting trading.

In the trading account trading system, 4 account modes are supported: Spot mode, Futures mode, Multi-currency margin mode, and Portfolio margin mode.

You need to set on the Web/App for the first set of every account mode.

Production Trading Services
The Production Trading URL:

REST: https://eea.okx.com
Public WebSocket: wss://wseea.okx.com:8443/ws/v5/public
Private WebSocket: wss://wseea.okx.com:8443/ws/v5/private
Business WebSocket: wss://wseea.okx.com:8443/ws/v5/business
Demo Trading Services
Currently, the API works for Demo Trading, but some functions are not supported, such as withdraw,deposit,purchase/redemption, etc.

The Demo Trading URL:

REST: https://eea.okx.com
Public WebSocket: wss://wseeapap.okx.com:8443/ws/v5/public
Private WebSocket: wss://wseeapap.okx.com:8443/ws/v5/private
Business WebSocket: wss://wseeapap.okx.com:8443/ws/v5/business
OKX account can be used for login on Demo Trading. If you already have an OKX account, you can log in directly.

Start API Demo Trading by the following steps:
Login OKX —> Trade —> Demo Trading —> Personal Center —> Demo Trading API -> Create Demo Trading API Key —> Start your Demo Trading

 Note: `x-simulated-trading: 1` needs to be added to the header of the Demo Trading request.
Http Header Example

Content-Type: application/json

OK-ACCESS-KEY: 37c541a1-****-****-****-10fe7a038418

OK-ACCESS-SIGN: leaVRETrtaoEQ3yI9qEtI1CZ82ikZ4xSG5Kj8gnl3uw=

OK-ACCESS-PASSPHRASE: 1****6

OK-ACCESS-TIMESTAMP: 2020-03-28T12:21:41.274Z

x-simulated-trading: 1


**Transaction Timeouts**
Orders may not be processed in time due to network delay or busy OKX servers. You can configure the expiry time of the request using expTime if you want the order request to be discarded after a specific time.

If expTime is specified in the requests for Place (multiple) orders or Amend (multiple) orders, the request will not be processed if the current system time of the server is after the expTime.

REST API
Set the following parameters in the request header

| Parameter | Type   | Required | Description                                                          |
|-----------|--------|----------|----------------------------------------------------------------------|
| expTime   | String | No       | Request effective deadline. Unix timestamp in milliseconds, e.g., 1597026383085 |

The following endpoints are supported:

Place order
Place multiple orders
Amend order
Amend multiple orders
POST / Place sub order under signal bot trading


Request Example

curl -X 'POST' \
  'https://www.okx.com/api/v5/trade/order' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'OK-ACCESS-KEY: *****' \
  -H 'OK-ACCESS-SIGN: *****'' \
  -H 'OK-ACCESS-TIMESTAMP: *****'' \
  -H 'OK-ACCESS-PASSPHRASE: *****'' \
  -H 'expTime: 1597026383085' \   // request effective deadline
  -d '{
  "instId": "BTC-USDT",
  "tdMode": "cash",
  "side": "buy",
  "ordType": "limit",
  "px": "1000",
  "sz": "0.01"
}'
WebSocket
The following parameters are set in the request

| Parameter | Type   | Required | Description                                                          |
|-----------|--------|----------|----------------------------------------------------------------------|
| expTime   | String | No       | Request effective deadline. Unix timestamp in milliseconds, e.g., 1597026383085 |

The following endpoints are supported:

Place order
Place multiple orders
Amend order
Amend multiple orders
Request Example

{
    "id": "1512",
    "op": "order",
    "expTime":"1597026383085",  // request effective deadline
    "args": [{
        "side": "buy",
        "instId": "BTC-USDT",
        "tdMode": "isolated",
        "ordType": "market",
        "sz": "100"
    }]
}

**Rate Limits**

Our REST and WebSocket APIs use rate limits to protect our APIs against malicious usage so our trading platform can operate reliably and fairly.
When a request is rejected by our system due to rate limits, the system returns error code 50011 (Rate limit reached. Please refer to API documentation and throttle requests accordingly).
The rate limit is different for each endpoint. You can find the limit for each endpoint from the endpoint details. Rate limit definitions are detailed below:

WebSocket login and subscription rate limits are based on connection.

Public unauthenticated REST rate limits are based on IP address.

Private REST rate limits are based on User ID (sub-accounts have individual User IDs).

WebSocket order management rate limits are based on User ID (sub-accounts have individual User IDs).

Trading-related APIs
For Trading-related APIs (place order, cancel order, and amend order) the following conditions apply:

Rate limits are shared across the REST and WebSocket channels.

Rate limits for placing orders, amending orders, and cancelling orders are independent from each other.

Rate limits are defined on the Instrument ID level (except Options)

Rate limits for Options are defined based on the Instrument Family level. Refer to the Get instruments endpoint to view Instrument Family information.

Rate limits for a multiple order endpoint and a single order endpoint are also independent, with the exception being when there is only one order sent to a multiple order endpoint, the order will be counted as a single order and adopt the single order rate limit.

Sub-account rate limit
At the sub-account level, we allow a maximum of 1000 order requests per 2 seconds. Only new order requests and amendment order requests will be counted towards this limit. The limit encompasses all requests from the endpoints below. For batch order requests consisting of multiple orders, each order will be counted individually. Error code 50061 is returned when the sub-account rate limit is exceeded. The existing rate limit rule per instrument ID remains unchanged and the existing rate limit and sub-account rate limit will operate in parallel. If clients require a higher rate limit, clients can trade via multiple sub-accounts.



Fill ratio based sub-account rate limit
This is only applicable to >= VIP5 customers.
As an incentive for more efficient trading, the exchange will offer a higher sub-account rate limit to clients with a high trade fill ratio.

The exchange calculates two ratios based on the transaction data from the past 7 days at 00:00 UTC.

Sub-account fill ratio: This ratio is determined by dividing (the trade volume in USDT of the sub-account) by (sum of (new and amendment request count per symbol * symbol multiplier) of the sub-account). Note that the master trading account itself is also considered as a sub-account in this context.
Master account aggregated fill ratio: This ratio is calculated by dividing (the trade volume in USDT on the master account level) by (the sum (new and amendment count per symbol * symbol multiplier] of all sub-accounts).


The symbol multiplier allows for fine-tuning the weight of each symbol. A smaller symbol multiplier (<1) is used for smaller pairs that require more updates per trading volume. All instruments have a default symbol multiplier, and some instruments will have overridden symbol multipliers.
| InstType           | Override rule          | Overridden symbol multiplier | Default symbol multiplier |
|-------------------|----------------------|-----------------------------|--------------------------|
| Perpetual Futures  | Per instrument ID     | 1                           |                           |
| Instrument ID:     |                      |                             |                          |
| BTC-USDT-SWAP      |                      | 0.2                         |                          |
| BTC-USD-SWAP       |                      | 0.2                         |                          |
| ETH-USDT-SWAP      |                      | 0.2                         |                          |
| ETH-USD-SWAP       |                      | 0.2                         |                          |
| Expiry Futures     | Per instrument Family | 0.3                         | 0.1                      |
| Instrument Family: |                      |                             |                          |
| BTC-USDT           |                      | 0.1                         |                          |
| BTC-USD            |                      | 0.1                         |                          |
| ETH-USDT           |                      | 0.1                         |                          |
| ETH-USD            |                      | 0.1                         |                          |
| Spot               | Per instrument ID     | 0.5                         | 0.1                      |
| Instrument ID:     |                      |                             |                          |
| BTC-USDT           |                      | 0.1                         |                          |
| ETH-USDT           |                      | 0.1                         |                          |
| Options            | Per instrument Family |                             | 0.1                      |

The fill ratio computation excludes block trading, spread trading, MMP and fiat orders for order count; and excludes block trading, spread trading for trade volume. Only successful order requests (sCode=0) are considered.




At 08:00 UTC, the system will use the maximum value between the sub-account fill ratio and the master account aggregated fill ratio based on the data snapshot at 00:00 UTC to determine the sub-account rate limit based on the table below. For broker (non-disclosed) clients, the system considers the sub-account fill ratio only.
| Tier   | Fill Ratio `[x <= ratio < y)` | Sub-account Rate Limit per 2s (new & amendment) |
| ------ | ----------------------------- | ----------------------------------------------- |
| Tier 1 | \[0,1)                        | 1,000                                           |
| Tier 2 | \[1,2)                        | 1,250                                           |
| Tier 3 | \[2,3)                        | 1,500                                           |
| Tier 4 | \[3,5)                        | 1,750                                           |
| Tier 5 | \[5,10)                       | 2,000                                           |
| Tier 6 | \[10,20)                      | 2,500                                           |
| Tier 7 | \[20,50)                      | 3,000                                           |
| Tier 8 | >=50                          | 10,000                                          |

If there is an improvement in the fill ratio and rate limit to be uplifted, the uplift will take effect immediately at 08:00 UTC. However, if the fill ratio decreases and the rate limit needs to be lowered, a one-day grace period will be granted, and the lowered rate limit will only be implemented on T+1 at 08:00 UTC. On T+1, if the fill ratio improves, the higher rate limit will be applied accordingly. In the event of client demotion to VIP4, their rate limit will be downgraded to Tier 1, accompanied by a one-day grace period.



If the 7-day trading volume of a sub-account is less than 1,000,000 USDT, the fill ratio of the master account will be applied to it.



For newly created sub-accounts, the Tier 1 rate limit will be applied at creation until T+1 8am UTC, at which the normal rules will be applied.



Block trading, spread trading, MMP and spot/margin orders are exempted from the sub-account rate limit.



The exchange offers GET / Account rate limit endpoint that provides ratio and rate limit data, which will be updated daily at 8am UTC. It will return the sub-account fill ratio, the master account aggregated fill ratio, current sub-account rate limit and sub-account rate limit on T+1 (applicable if the rate limit is going to be demoted).

The fill ratio and rate limit calculation example is shown below. Client has 3 accounts, symbol multiplier for BTC-USDT-SWAP = 1 and XRP-USDT = 0.1.

Account A (master account):
BTC-USDT-SWAP trade volume = 100 USDT, order count = 10;
XRP-USDT trade volume = 20 USDT, order count = 15;
Sub-account ratio = (100+20) / (10 * 1 + 15 * 0.1) = 10.4
Account B (sub-account):
BTC-USDT-SWAP trade volume = 200 USDT, order count = 100;
XRP-USDT trade volume = 20 USDT, order count = 30;
Sub-account ratio = (200+20) / (100 * 1 + 30 * 0.1) = 2.13
Account C (sub-account):
BTC-USDT-SWAP trade volume = 300 USDT, order count = 1000;
XRP-USDT trade volume = 20 USDT, order count = 45;
Sub-account ratio = (300+20) / (100 * 1 + 45 * 0.1) = 3.06
Master account aggregated fill ratio = (100+20+200+20+300+20) / (10 * 1 + 15 * 0.1 + 100 * 1 + 30 * 0.1 + 100 * 1 + 45 * 0.1) = 3.01
Rate limit of accounts
Account A = max(10.4, 3.01) = 10.4 -> 2500 order requests/2s
Account B = max(2.13, 3.01) = 3.01 -> 1750 order requests/2s
Account C = max(3.06, 3.01) = 3.06 -> 1750 order requests/2s
Best practices
If you require a higher request rate than our rate limit, you can set up different sub-accounts to batch request rate limits. We recommend this method for throttling or spacing out requests in order to maximize each accounts' rate limit and avoid disconnections or rejections.

**Trading Account**
The API endpoints of Account require authentication.

REST API
Get instruments
Retrieve available instruments info of current account.

Rate Limit: 20 requests per 2 seconds
Rate limit rule: User ID + Instrument Type
Permission: Read
HTTP Request
GET /api/v5/account/instruments

Request Example

GET /api/v5/account/instruments?instType=SPOT

Request Parameters
| Parameter  | Type   | Required    | Description                                                      |
| ---------- | ------ | ----------- | ---------------------------------------------------------------- |
| instType   | String | Yes         | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION             |
| uly        | String | Conditional | Underlying; required if instType=OPTION (or instFamily required) |
| instFamily | String | Conditional | Instrument family; required if instType=OPTION (or uly required) |
| instId     | String | No          | Instrument ID                                                    |

Request Example:

GET /api/v5/account/instruments?instType=SPOT


Response Example:

{
    "code": "0",
    "data": [
        {
            "auctionEndTime": "",
            "baseCcy": "BTC",
            "ctMult": "",
            "ctType": "",
            "ctVal": "",
            "ctValCcy": "",
            "contTdSwTime": "1704876947000",
            "expTime": "",
            "futureSettlement": false,
            "instFamily": "",
            "instId": "BTC-EUR",
            "instType": "SPOT",
            "lever": "",
            "listTime": "1704876947000",
            "lotSz": "0.00000001",
            "maxIcebergSz": "9999999999.0000000000000000",
            "maxLmtAmt": "1000000",
            "maxLmtSz": "9999999999",
            "maxMktAmt": "1000000",
            "maxMktSz": "1000000",
            "maxStopSz": "1000000",
            "maxTriggerSz": "9999999999.0000000000000000",
            "maxTwapSz": "9999999999.0000000000000000",
            "minSz": "0.00001",
            "optType": "",
            "openType": "call_auction",
            "quoteCcy": "EUR",
            "tradeQuoteCcyList": [
                "EUR"
            ],
            "settleCcy": "",
            "state": "live",
            "ruleType": "normal",
            "stk": "",
            "tickSz": "1",
            "uly": ""
        }
    ],
    "msg": ""
}


Response Parameters
| Parameter           | Type               | Description                                                                 |
|--------------------|------------------|-----------------------------------------------------------------------------|
| instType           | String            | Instrument type                                                              |
| instId             | String            | Instrument ID, e.g. BTC-USD-SWAP                                            |
| uly                | String            | Underlying, e.g. BTC-USD. Only applicable to MARGIN/FUTURES/SWAP/OPTION     |
| instFamily         | String            | Instrument family, e.g. BTC-USD. Only applicable to MARGIN/FUTURES/SWAP/OPTION |
| baseCcy            | String            | Base currency, e.g. BTC in BTC-USDT. Only applicable to SPOT/MARGIN         |
| quoteCcy           | String            | Quote currency, e.g. USDT in BTC-USDT. Only applicable to SPOT/MARGIN       |
| settleCcy          | String            | Settlement and margin currency, e.g. BTC. Only applicable to FUTURES/SWAP/OPTION |
| ctVal              | String            | Contract value. Only applicable to FUTURES/SWAP/OPTION                       |
| ctMult             | String            | Contract multiplier. Only applicable to FUTURES/SWAP/OPTION                  |
| ctValCcy           | String            | Contract value currency. Only applicable to FUTURES/SWAP/OPTION              |
| optType            | String            | Option type, C: Call P: Put. Only applicable to OPTION                       |
| stk                | String            | Strike price. Only applicable to OPTION                                      |
| listTime           | String            | Listing time, Unix timestamp in milliseconds                                 |
| auctionEndTime     | String            | End time of call auction, Unix timestamp in milliseconds. Only for SPOT call auctions; return "" otherwise (deprecated) |
| contTdSwTime       | String            | Continuous trading switch time. Only for SPOT/MARGIN with call auction/prequote; return "" otherwise |
| openType           | String            | Open type: fix_price: fix price opening, pre_quote: pre-quote, call_auction: call auction. Only applicable to SPOT/MARGIN; return "" otherwise |
| expTime            | String            | Expiry time. Applicable to SPOT/MARGIN/FUTURES/SWAP/OPTION. For FUTURES/OPTION, it is natural delivery/exercise time |
| lever              | String            | Max leverage. Not applicable to SPOT, OPTION                                 |
| tickSz             | String            | Tick size, e.g. 0.0001. For OPTION, minimum tick size among tick bands      |
| lotSz              | String            | Lot size. Number of contracts for derivatives; quantity in base currency for SPOT/MARGIN |
| minSz              | String            | Minimum order size. Number of contracts for derivatives; quantity in base currency for SPOT/MARGIN |
| ctType             | String            | Contract type: linear (linear contract), inverse (inverse contract). Only for FUTURES/SWAP |
| state              | String            | Instrument status: live, suspend, preopen, test                               |
| ruleType           | String            | Trading rule types: normal, pre_market                                       |
| maxLmtSz           | String            | Max order quantity of a single limit order                                   |
| maxMktSz           | String            | Max order quantity of a single market order                                  |
| maxLmtAmt          | String            | Max USD amount for a single limit order                                      |
| maxMktAmt          | String            | Max USD amount for a single market order. Only applicable to SPOT/MARGIN     |
| maxTwapSz          | String            | Max order quantity of a single TWAP order. Minimum order = minSz*2           |
| maxIcebergSz       | String            | Max order quantity of a single iceberg order                                  |
| maxTriggerSz       | String            | Max order quantity of a single trigger order                                  |
| maxStopSz          | String            | Max order quantity of a single stop market order                              |
| futureSettlement   | Boolean           | Whether daily settlement for expiry feature is enabled. Applicable to FUTURES cross |
| tradeQuoteCcyList  | Array of strings  | List of quote currencies available for trading, e.g. ["USD", "USDC"]         |

 listTime and contTdSwTime
For spot symbols listed through a call auction or pre-open, listTime represents the start time of the auction or pre-open, and contTdSwTime indicates the end of the auction or pre-open and the start of continuous trading. For other scenarios, listTime will mark the beginning of continuous trading, and contTdSwTime will return an empty value "".
 state
The state will always change from `preopen` to `live` when the listTime is reached.
When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument will not be available.

Get balance
Retrieve a list of assets (with non-zero balance), remaining balance, and available amount in the trading account.

Rate Limit: 10 requests per 2 seconds
Rate limit rule: User ID
Permission: Read
HTTP Request

GET /api/v5/account/balance

Request Example

# Get the balance of all assets in the account
GET /api/v5/account/balance

# Get the balance of BTC and ETH assets in the account
GET /api/v5/account/balance?ccy=BTC,ETH

Request Parameters
| Parameter | Type   | Required | Description                                                                 |
|-----------|--------|---------|-----------------------------------------------------------------------------|
| ccy       | String | No      | Single currency or multiple currencies (no more than 20) separated with comma, e.g. BTC or BTC,ETH |


Response Example

{
    "code": "0",
    "data": [
        {
            "adjEq": "55415.624719833286",
            "availEq": "624719833286",
            "borrowFroz": "0",
            "details": [
                {
                    "autoLendStatus": "off",
                    "autoLendMtAmt": "0",
                    "availBal": "4834.317093622894",
                    "availEq": "4834.3170936228935",
                    "borrowFroz": "0",
                    "cashBal": "4850.435693622894",
                    "ccy": "USDT",
                    "crossLiab": "0",
                    "collateralEnabled": false,
                    "collateralRestrict": false,
                    "colBorrAutoConversion": "0",
                    "disEq": "4991.542013297616",
                    "eq": "4992.890093622894",
                    "eqUsd": "4991.542013297616",
                    "smtSyncEq": "0",
                    "spotCopyTradingEq": "0",
                    "fixedBal": "0",
                    "frozenBal": "158.573",
                    "imr": "",
                    "interest": "0",
                    "isoEq": "0",
                    "isoLiab": "0",
                    "isoUpl": "0",
                    "liab": "0",
                    "maxLoan": "0",
                    "mgnRatio": "",
                    "mmr": "",
                    "notionalLever": "",
                    "ordFrozen": "0",
                    "rewardBal": "0",
                    "spotInUseAmt": "",
                    "clSpotInUseAmt": "",
                    "maxSpotInUse": "",
                    "spotIsoBal": "0",
                    "stgyEq": "150",
                    "twap": "0",
                    "uTime": "1705449605015",
                    "upl": "-7.545600000000006",
                    "uplLiab": "0",
                    "spotBal": "",
                    "openAvgPx": "",
                    "accAvgPx": "",
                    "spotUpl": "",
                    "spotUplRatio": "",
                    "totalPnl": "",
                    "totalPnlRatio": ""
                }
            ],
            "imr": "0",
            "isoEq": "0",
            "mgnRatio": "",
            "mmr": "0",
            "notionalUsd": "0",
            "notionalUsdForBorrow": "0",
            "notionalUsdForFutures": "0",
            "notionalUsdForOption": "0",
            "notionalUsdForSwap": "0",
            "ordFroz": "",
            "totalEq": "55837.43556134779",
            "uTime": "1705474164160",
            "upl": "0",
        }
    ],
    "msg": ""
}

| Parameters | Types | Description |
|------------|-------|-------------|
| uTime | String | Update time of account information, millisecond format of Unix timestamp, e.g. 1597026383085 |
| totalEq | String | The total amount of equity in USD |
| isoEq | String | Isolated margin equity in USD. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| adjEq | String | Adjusted / Effective equity in USD. The net fiat value of the assets in the account that can provide margins for spot, expiry futures, perpetual futures and options under the cross-margin mode. In multi-ccy or PM mode, the asset and margin requirement will all be converted to USD value to process the order check or liquidation. Due to the volatility of each currency market, our platform calculates the actual USD value of each currency based on discount rates to balance market risks. Applicable to Multi-currency margin and Portfolio margin |
| availEq | String | Account level available equity, excluding currencies that are restricted due to the collateralized borrowing limit. Applicable to Multi-currency margin / Portfolio margin |
| ordFroz | String | Cross margin frozen for pending orders in USD. Only applicable to Multi-currency margin |
| imr | String | Initial margin requirement in USD. The sum of initial margins of all open positions and pending orders under cross-margin mode in USD. Applicable to Multi-currency margin / Portfolio margin |
| mmr | String | Maintenance margin requirement in USD. The sum of maintenance margins of all open positions and pending orders under cross-margin mode in USD. Applicable to Multi-currency margin / Portfolio margin |
| borrowFroz | String | Potential borrowing IMR of the account in USD. Only applicable to Multi-currency margin / Portfolio margin. It is "" for other margin modes. |
| mgnRatio | String | Maintenance margin ratio in USD. Applicable to Multi-currency margin / Portfolio margin |
| notionalUsd | String | Notional value of positions in USD. Applicable to Multi-currency margin / Portfolio margin |
| notionalUsdForBorrow | String | Notional value for Borrow in USD. Applicable to Spot mode / Multi-currency margin / Portfolio margin |
| notionalUsdForSwap | String | Notional value of positions for Perpetual Futures in USD. Applicable to Multi-currency margin / Portfolio margin |
| notionalUsdForFutures | String | Notional value of positions for Expiry Futures in USD. Applicable to Multi-currency margin / Portfolio margin |
| notionalUsdForOption | String | Notional value of positions for Option in USD. Applicable to Spot mode / Multi-currency margin / Portfolio margin |
| upl | String | Cross-margin info of unrealized profit and loss at the account level in USD. Applicable to Multi-currency margin / Portfolio margin |
| details | Array of objects | Detailed asset information in all currencies |
| > ccy | String | Currency |
| > eq | String | Equity of currency |
| > cashBal | String | Cash balance |
| > uTime | String | Update time of currency balance information, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| > isoEq | String | Isolated margin equity of currency. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| > availEq | String | Available equity of currency. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| > disEq | String | Discount equity of currency in USD. Applicable to Spot mode (enabled spot borrow) / Multi-currency margin / Portfolio margin |
| > fixedBal | String | Frozen balance for Dip Sniper and Peak Sniper |
| > availBal | String | Available balance of currency |
| > frozenBal | String | Frozen balance of currency |
| > ordFrozen | String | Margin frozen for open orders. Applicable to Spot mode / Futures mode / Multi-currency margin |
| > liab | String | Liabilities of currency. Positive value, e.g. 21625.64. Applicable to Multi-currency margin / Portfolio margin |
| > upl | String | Sum of unrealized profit & loss of all margin and derivatives positions of currency. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| > uplLiab | String | Liabilities due to Unrealized loss of currency. Applicable to Multi-currency margin / Portfolio margin |
| > crossLiab | String | Cross liabilities of currency. Applicable to Multi-currency margin / Portfolio margin |
| > isoLiab | String | Isolated liabilities of currency. Applicable to Multi-currency margin / Portfolio margin |
| > mgnRatio | String | Cross maintenance margin ratio of currency. Index for measuring risk of a certain asset in the account. Applicable to Futures mode and when there is cross position |
| > imr | String | Cross initial margin requirement at the currency level. Applicable to Futures mode and when there is cross position |
| > mmr | String | Cross maintenance margin requirement at the currency level. Applicable to Futures mode and when there is cross position |
| > interest | String | Accrued interest of currency. Positive value, e.g. 9.01. Applicable to Multi-currency margin / Portfolio margin |
| > twap | String | Risk indicator of auto liability repayment. Levels 0–5, higher means higher chance of auto repayment trigger. Applicable to Multi-currency margin / Portfolio margin |
| > maxLoan | String | Max loan of currency. Applicable to cross of Spot mode / Multi-currency margin / Portfolio margin |
| > eqUsd | String | Equity in USD of currency |
| > borrowFroz | String | Potential borrowing IMR of currency in USD. Applicable to Multi-currency margin / Portfolio margin; "" for other margin modes |
| > notionalLever | String | Leverage of currency. Applicable to Futures mode |
| > stgyEq | String | Strategy equity |
| > isoUpl | String | Isolated unrealized profit and loss of currency. Applicable to Futures mode / Multi-currency margin / Portfolio margin |
| > spotInUseAmt | String | Spot in use amount. Applicable to Portfolio margin |
| > spotIsoBal | String | Spot isolated balance. Applicable to copy trading and to Spot mode / Futures mode |
| > spotBal | String | Spot balance. Unit is currency, e.g. BTC |
| > openAvgPx | String | Spot average cost price. Unit is USD |
| > accAvgPx | String | Spot accumulated cost price. Unit is USD |
| > spotUpl | String | Spot unrealized profit and loss. Unit is USD |
| > spotUplRatio | String | Spot unrealized profit and loss ratio |
| > totalPnl | String | Spot accumulated profit and loss. Unit is USD |
| > totalPnlRatio | String | Spot accumulated profit and loss ratio |
| > collateralEnabled | Boolean | true: Collateral enabled; false: Collateral disabled. Applicable to Multi-currency margin |
| > collateralRestrict | Boolean | Platform-level collateralized borrow restriction (true/false) |
| > colBorrAutoConversion | String | Indicator of forced repayment when collateralized borrowing on a crypto reaches platform limit. Levels 1–5; default 0 means no risk, 5 means undergoing auto conversion. Applicable to Spot mode / Futures mode / Multi-currency margin / Portfolio margin |
| > autoLendStatus | String | Auto lend status: unsupported, off, pending, active |
| > autoLendMtAmt | String | Auto lend matched amount. Returns "0" unless active |

"" will be returned for inapplicable fields under the current account level.
 The currency details will not be returned when cashBal and eq is both 0.

 Distribution of applicable fields under each account level are as follows:

 | Parameter                 | Spot mode | Futures mode | Multi-currency margin mode | Portfolio margin mode |
| ------------------------- | --------- | ------------ | -------------------------- | --------------------- |
| **uTime**                 | Yes       | Yes          | Yes                        | Yes                   |
| **totalEq**               | Yes       | Yes          | Yes                        | Yes                   |
| **isoEq**                 |           | Yes          | Yes                        | Yes                   |
| **adjEq**                 |           |              | Yes                        | Yes                   |
| **availEq**               |           |              | Yes                        | Yes                   |
| **ordFroz**               |           |              | Yes                        | Yes                   |
| **imr**                   |           |              | Yes                        | Yes                   |
| **mmr**                   |           |              | Yes                        | Yes                   |
| **mgnRatio**              |           |              | Yes                        | Yes                   |
| **notionalUsd**           |           |              | Yes                        | Yes                   |
| **notionalUsdForSwap**    |           |              | Yes                        | Yes                   |
| **notionalUsdForFutures** |           |              | Yes                        | Yes                   |
| **notionalUsdForOption**  | Yes       |              | Yes                        | Yes                   |
| **notionalUsdForBorrow**  | Yes       |              | Yes                        | Yes                   |
| **upl**                   |           |              | Yes                        | Yes                   |
| **details**               |           |              | Yes                        | Yes                   |
| **ccy**                   | Yes       | Yes          | Yes                        | Yes                   |
| **eq**                    | Yes       | Yes          | Yes                        | Yes                   |
| **cashBal**               | Yes       | Yes          | Yes                        | Yes                   |
| **uTime (details)**       | Yes       | Yes          | Yes                        | Yes                   |
| **isoEq (details)**       |           | Yes          | Yes                        | Yes                   |
| **availEq (details)**     |           | Yes          | Yes                        | Yes                   |
| **disEq**                 | Yes       |              | Yes                        | Yes                   |
| **availBal**              | Yes       | Yes          | Yes                        | Yes                   |
| **frozenBal**             | Yes       | Yes          | Yes                        | Yes                   |
| **ordFrozen**             | Yes       | Yes          | Yes                        | Yes                   |
| **liab**                  |           |              | Yes                        | Yes                   |
| **upl (details)**         |           | Yes          | Yes                        | Yes                   |
| **uplLiab**               |           |              | Yes                        | Yes                   |
| **crossLiab**             |           |              | Yes                        | Yes                   |
| **isoLiab**               |           |              | Yes                        | Yes                   |
| **mgnRatio (details)**    |           | Yes          |                            |                       |
| **interest**              |           |              | Yes                        | Yes                   |
| **twap**                  |           |              | Yes                        | Yes                   |
| **maxLoan**               | Yes       |              | Yes                        | Yes                   |
| **eqUsd**                 | Yes       | Yes          | Yes                        | Yes                   |
| **notionalLever**         |           | Yes          |                            |                       |
| **stgyEq**                | Yes       | Yes          | Yes                        | Yes                   |
| **isoUpl**                |           | Yes          | Yes                        | Yes                   |
| **spotInUseAmt**          |           |              |                            | Yes                   |
| **spotIsoBal**            | Yes       | Yes          |                            |                       |
| **imr (details)**         |           | Yes          |                            |                       |
| **mmr (details)**         |           | Yes          |                            |                       |
| **spotBal**               | Yes       | Yes          | Yes                        | Yes                   |
| **openAvgPx**             | Yes       | Yes          | Yes                        | Yes                   |
| **accAvgPx**              | Yes       | Yes          | Yes                        | Yes                   |
| **spotUpl**               | Yes       | Yes          | Yes                        | Yes                   |
| **spotUplRatio**          | Yes       | Yes          | Yes                        | Yes                   |
| **totalPnl**              | Yes       | Yes          | Yes                        | Yes                   |
| **totalPnlRatio**         | Yes       | Yes          | Yes                        | Yes                   |







# Get bills details (last 7 days)

Retrieve the bills of the account. The bill refers to all transaction records that result in changing the balance of an account. Pagination is supported, and the response is sorted with the most recent first. This endpoint can retrieve data from the last 7 days.

**Rate Limit:** 5 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

## HTTP Request
```
GET /api/v5/account/bills
```

## Request Example

```
GET /api/v5/account/bills

GET /api/v5/account/bills?instType=SPOT
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instType | String | No | Instrument type<br>SPOT |
| ccy | String | No | Bill currency |
| type | String | No | Bill type<br>1: Transfer<br>2: Trade<br>27: Convert<br>30: Simple trade |
| subType | String | No | Bill subtype<br>1: Buy<br>2: Sell<br>11: Transfer in<br>12: Transfer out<br><br>318: Convert in<br>319: Convert out<br>320: Simple buy<br>321: Simple sell |
| after | String | No | Pagination of data to return records earlier than the requested bill ID. |
| before | String | No | Pagination of data to return records newer than the requested bill ID. |
| begin | String | No | Filter with a begin timestamp. Unix timestamp format in milliseconds, e.g. 1597026383085 |
| end | String | No | Filter with an end timestamp. Unix timestamp format in milliseconds, e.g. 1597026383085 |
| limit | String | No | Number of results per request. The maximum is 100. The default is 100. |

## Response Example

```
{
    "code": "0",
    "msg": "",
    "data": [{
        "bal": "8694.2179403378290202",
        "balChg": "0.0219338232210000",
        "billId": "623950854533513219",
        "ccy": "USDT",
        "clOrdId": "",
        "execType": "T",
        "fee": "-0.000021955779",
        "fillFwdPx": "",
        "fillIdxPx": "27104.1",
        "fillMarkPx": "",
        "fillMarkVol": "",
        "fillPxUsd": "",
        "fillPxVol": "",
        "fillTime": "1695033476166",
        "from": "",
        "instId": "BTC-USDT",
        "instType": "SPOT",
        "interest": "0",
        "mgnMode": "isolated",
        "notes": "",
        "ordId": "623950854525124608",
        "pnl": "0",
        "posBal": "0",
        "posBalChg": "0",
        "px": "27105.9",
        "subType": "1",
        "sz": "0.021955779",
        "tag": "",
        "to": "",
        "tradeId": "586760148",
        "ts": "1695033476167",
        "type": "2"
    }]
} 
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instType | String | Instrument type |
| billId | String | Bill ID |
| type | String | Bill type |
| subType | String | Bill subtype |
| ts | String | The time when the balance complete update, Unix timestamp format in milliseconds, e.g.1597026383085 |
| balChg | String | Change in balance amount at the account level |
| posBalChg | String | Change in balance amount at the position level |
| bal | String | Balance at the account level |
| posBal | String | Balance at the position level |
| sz | String | Quantity<br>For FUTURES/SWAP/OPTION, it is fill quantity or position quantity, the unit is contract. The value is always positive.<br>For other scenarios. the unit is account balance currency(ccy). |
| px | String | Price which related to subType<br>Trade filled price for<br>1: Buy 2: Sell |
| ccy | String | Account balance currency |
| pnl | String | Profit and loss |
| fee | String | Fee<br>Negative number represents the user transaction fee charged by the platform.<br>Positive number represents rebate. |
| mgnMode | String | Margin mode<br>isolated cross cash<br>When bills are not generated by trading, the field returns "" |
| instId | String | Instrument ID, e.g. BTC-USDT |
| ordId | String | Order ID |
| execType | String | Liquidity taker or maker<br>T: taker<br>M: maker |
| from | String | The remitting account<br>6: Funding account<br>18: Trading account<br>Only applicable to transfer. When bill type is not transfer, the field returns "". |
| to | String | The beneficiary account<br>6: Funding account<br>18: Trading account<br>Only applicable to transfer. When bill type is not transfer, the field returns "". |
| notes | String | Notes |
| interest | String | Interest |
| tag | String | Order tag |
| fillTime | String | Last filled time |
| tradeId | String | Last traded ID |
| clOrdId | String | Client Order ID as assigned by the client<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| fillIdxPx | String | Index price at the moment of trade execution<br>For cross currency spot pairs, it returns baseCcy-USDT index price. For example, for LTC-ETH, this field returns the index price of LTC-USDT. |
| fillMarkPx | String | Mark price when filled<br>Applicable to FUTURES/SWAP/OPTIONS, return "" for other instrument types |
| fillPxVol | String | Implied volatility when filled<br>Only applicable to options; return "" for other instrument types |
| fillPxUsd | String | Options price when filled, in the unit of USD<br>Only applicable to options; return "" for other instrument types |
| fillMarkVol | String | Mark volatility when filled<br>Only applicable to options; return "" for other instrument types |
| fillFwdPx | String | Forward price when filled<br>Only applicable to options; return "" for other instrument types |

# Get bills details (last 3 months)

Retrieve the account's bills. The bill refers to all transaction records that result in changing the balance of an account. Pagination is supported, and the response is sorted with most recent first. This endpoint can retrieve data from the last 3 months.

**Rate Limit:** 5 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

## HTTP Request
```
GET /api/v5/account/bills-archive
```

## Request Example

```
GET /api/v5/account/bills-archive

GET /api/v5/account/bills-archive?instType=SPOT
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instType | String | No | Instrument type<br>SPOT |
| ccy | String | No | Bill currency |
| type | String | No | Bill type<br>1: Transfer<br>2: Trade<br>6: Margin transfer |
| subType | String | No | Bill subtype<br>1: Buy<br>2: Sell<br>11: Transfer in<br>12: Transfer out<br>236: Easy convert in<br>237: Easy convert out |
| after | String | No | Pagination of data to return records earlier than the requested bill ID. |
| before | String | No | Pagination of data to return records newer than the requested bill ID. |
| begin | String | No | Filter with a begin timestamp. Unix timestamp format in milliseconds, e.g. 1597026383085 |
| end | String | No | Filter with an end timestamp. Unix timestamp format in milliseconds, e.g. 1597026383085 |
| limit | String | No | Number of results per request. The maximum is 100. The default is 100. |

## Response Example

```
{
    "code": "0",
    "msg": "",
    "data": [{
        "bal": "8694.2179403378290202",
        "balChg": "0.0219338232210000",
        "billId": "623950854533513219",
        "ccy": "USDT",
        "clOrdId": "",
        "execType": "T",
        "fee": "-0.000021955779",
        "fillFwdPx": "",
        "fillIdxPx": "27104.1",
        "fillMarkPx": "",
        "fillMarkVol": "",
        "fillPxUsd": "",
        "fillPxVol": "",
        "fillTime": "1695033476166",
        "from": "",
        "instId": "BTC-USDT",
        "instType": "SPOT",
        "interest": "0",
        "mgnMode": "isolated",
        "notes": "",
        "ordId": "623950854525124608",
        "pnl": "0",
        "posBal": "0",
        "posBalChg": "0",
        "px": "27105.9",
        "subType": "1",
        "sz": "0.021955779",
        "tag": "",
        "to": "",
        "tradeId": "586760148",
        "ts": "1695033476167",
        "type": "2"
    }]
} 
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instType | String | Instrument type |
| billId | String | Bill ID |
| type | String | Bill type |
| subType | String | Bill subtype |
| ts | String | The time when the balance complete update, Unix timestamp format in milliseconds, e.g.1597026383085 |
| balChg | String | Change in balance amount at the account level |
| posBalChg | String | Change in balance amount at the position level |
| bal | String | Balance at the account level |
| posBal | String | Balance at the position level |
| sz | String | Quantity<br>For FUTURES/SWAP/OPTION, it is fill quantity or position quantity, the unit is contract. The value is always positive.<br>For other scenarios. the unit is account balance currency(ccy). |
| px | String | Price which related to subType<br>Trade filled price for<br>1: Buy 2: Sell |
| ccy | String | Account balance currency |
| pnl | String | Profit and loss |
| fee | String | Fee<br>Negative number represents the user transaction fee charged by the platform.<br>Positive number represents rebate. |
| mgnMode | String | Margin mode<br>isolated cross cash<br>When bills are not generated by trading, the field returns "" |
| instId | String | Instrument ID, e.g. BTC-USDT |
| ordId | String | Order ID<br>When bill type is not trade, the field returns "" |
| execType | String | Liquidity taker or maker<br>T: taker<br>M: maker |
| from | String | The remitting account<br>6: Funding account<br>18: Trading account<br>Only applicable to transfer. When bill type is not transfer, the field returns "". |
| to | String | The beneficiary account<br>6: Funding account<br>18: Trading account<br>Only applicable to transfer. When bill type is not transfer, the field returns "". |
| notes | String | Notes |
| interest | String | Interest |
| tag | String | Order tag |
| fillTime | String | Last filled time |
| tradeId | String | Last traded ID |
| clOrdId | String | Client Order ID as assigned by the client<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| fillIdxPx | String | Index price at the moment of trade execution<br>For cross currency spot pairs, it returns baseCcy-USDT index price. For example, for LTC-ETH, this field returns the index price of LTC-USDT. |
| fillMarkPx | String | Mark price when filled<br>Applicable to FUTURES/SWAP/OPTIONS, return "" for other instrument types |
| fillPxVol | String | Implied volatility when filled<br>Only applicable to options; return "" for other instrument types |
| fillPxUsd | String | Options price when filled, in the unit of USD<br>Only applicable to options; return "" for other instrument types |
| fillMarkVol | String | Mark volatility when filled<br>Only applicable to options; return "" for other instrument types |
| fillFwdPx | String | Forward price when filled<br>Only applicable to options; return "" for other instrument types |

💡 **Funding Fee expense (subType = 173)**  
You may refer to "pnl" for the fee payment

# Apply bills details (since 2021)

Apply for bill data since 1 February, 2021 except for the current quarter.

**Rate Limit:** 12 requests per day  
**Rate limit rule:** User ID  
**Permission:** Read

## HTTP Request
```
POST /api/v5/account/bills-history-archive
```

## Request Example

```
POST /api/v5/account/bills-history-archive
body
{
    "year":"2023",
    "quarter":"Q1"
}
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| year | String | Yes | 4 digits year |
| quarter | String | Yes | Quarter, valid value is Q1, Q2, Q3, Q4 |

## Response Example

```
{
    "code": "0",
    "data": [
        {
            "result": "true",
            "ts": "1646892328000"
        }
    ],
    "msg": ""
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| result | String | Whether there is already a download link for this section<br>true: Existed, can check from "Get bills details (since 2021)".<br>false: Does not exist and is generating, can check the download link after 2 hours<br>The data of file is in reverse chronological order using billId. |
| ts | String | The first request time when the server receives. Unix timestamp format in milliseconds, e.g. 1597026383085 |

💡 **The rule introduction, only applicable to the file generated after 11 October, 2024**
1. Taking 2024 Q2 as an example. The date range are [2024-07-01, 2024-10-01). The begin date is included, The end date is excluded.
2. The data of file is in reverse chronological order using `billId`

💡 Check the file link from the "Get bills details (since 2021)" endpoint in 2 hours to allow for data generation.  
During peak demand, data generation may take longer. If the file link is still unavailable after 3 hours, reach out to customer support for assistance.

💡 It is only applicable to the data from the unified account.

# Get bills details (since 2021)

Apply for bill data since 1 February, 2021 except for the current quarter.

**Rate Limit:** 10 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

## HTTP Request
```
GET /api/v5/account/bills-history-archive
```

## Response Example

```
GET /api/v5/account/bills-history-archive?year=2023&quarter=Q4
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| year | String | Yes | 4 digits year |
| quarter | String | Yes | Quarter, valid value is Q1, Q2, Q3, Q4 |

## Response Example

```
{
    "code": "0",
    "data": [
        {
            "fileHref": "http://xxx",
            "state": "finished",
            "ts": "1646892328000"
        }
    ],
    "msg": ""
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| fileHref | String | Download file link.<br>The expiration of every link is 5 and a half hours. If you already apply the files for the same quarter, then it don't need to apply again within 30 days. |
| ts | String | The first request time when the server receives. Unix timestamp format in milliseconds, e.g. 1597026383085 |
| state | String | Download link status<br>"finished" "ongoing" "failed": Failed, please apply again |

💡 It is only applicable to the data from the unified account.

## Field descriptions in the decompressed CSV file

| Parameter | Type | Description |
|-----------|------|-------------|
| instType | String | Instrument type |
| billId | String | Bill ID |
| subType | String | Bill subtype |
| ts | String | The time when the balance complete update, Unix timestamp format in milliseconds, e.g.1597026383085 |
| balChg | String | Change in balance amount at the account level |
| posBalChg | String | Change in balance amount at the position level |
| bal | String | Balance at the account level |
| posBal | String | Balance at the position level |
| sz | String | Quantity |
| px | String | Price which related to subType<br>Trade filled price for<br>1: Buy 2: Sell 3: Open long 4: Open short 5: Close long 6: Close short 204: block trade buy 205: block trade sell 206: block trade open long 207: block trade open short 208: block trade close long 209: block trade close short 114: Forced repayment buy 115: Forced repayment sell<br><br>Liquidation Price for<br>100: Partial liquidation close long 101: Partial liquidation close short 102: Partial liquidation buy 103: Partial liquidation sell 104: Liquidation long 105: Liquidation short 106: Liquidation buy 107: Liquidation sell 16: Repay forcibly 17: Repay interest by borrowing forcibly 110: Liquidation transfer in 111: Liquidation transfer out<br><br>Delivery price for<br>112: Delivery long 113: Delivery short<br><br>Exercise price for<br>170: Exercised 171: Counterparty exercised 172: Expired OTM<br><br>Mark price for<br>173: Funding fee expense 174: Funding fee income |
| ccy | String | Account balance currency |
| pnl | String | Profit and loss |
| fee | String | Fee<br>Negative number represents the user transaction fee charged by the platform.<br>Positive number represents rebate.<br>Trading fee rule |
| mgnMode | String | Margin mode<br>isolated cross cash<br>When bills are not generated by trading, the field returns "" |
| instId | String | Instrument ID, e.g. BTC-USDT |
| ordId | String | Order ID<br>Return order ID when the type is 2/5/9<br>Return "" when there is no order. |
| execType | String | Liquidity taker or maker<br>T: taker M: maker |
| interest | String | Interest |
| tag | String | Order tag |
| fillTime | String | Last filled time |
| tradeId | String | Last traded ID |
| clOrdId | String | Client Order ID as assigned by the client<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| fillIdxPx | String | Index price at the moment of trade execution<br>For cross currency spot pairs, it returns baseCcy-USDT index price. For example, for LTC-ETH, this field returns the index price of LTC-USDT. |
| fillMarkPx | String | Mark price when filled<br>Applicable to FUTURES/SWAP/OPTIONS, return "" for other instrument types |
| fillPxVol | String | Implied volatility when filled<br>Only applicable to options; return "" for other instrument types |
| fillPxUsd | String | Options price when filled, in the unit of USD<br>Only applicable to options; return "" for other instrument types |
| fillMarkVol | String | Mark volatility when filled<br>Only applicable to options; return "" for other instrument types |
| fillFwdPx | String | Forward price when filled<br>Only applicable to options; return "" for other instrument types |

# Get account configuration

Retrieve current account configuration.

**Rate Limit:** 5 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

## HTTP Request
```
GET /api/v5/account/config
```

## Request Example

```
GET /api/v5/account/config
```

## Request Parameters
none

## Response Example

```
{
    "code": "0",
    "data": [
        {
            "acctLv": "2",
            "acctStpMode": "cancel_maker",
            "autoLoan": false,
            "ctIsoMode": "automatic",
            "enableSpotBorrow": false,
            "greeksType": "PA",
            "ip": "",
            "type": "0",
            "kycLv": "3",
            "label": "v5 test",
            "level": "Lv1",
            "levelTmp": "",
            "liquidationGear": "-1",
            "mainUid": "44705892343619584",
            "mgnIsoMode": "automatic",
            "opAuth": "1",
            "perm": "read_only,withdraw,trade",
            "posMode": "long_short_mode",
            "roleType": "0",
            "spotBorrowAutoRepay": false,
            "spotOffsetType": "",
            "spotRoleType": "0",
            "spotTraderInsts": [],
            "traderInsts": [],
            "uid": "44705892343619584"
        }
    ],
    "msg": ""
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| uid | String | Account ID of current request. |
| mainUid | String | Main Account ID of current request.<br>The current request account is main account if uid = mainUid.<br>The current request account is sub-account if uid != mainUid. |
| acctLv | String | Account mode<br>1: Spot mode<br>2: Futures mode<br>3: Multi-currency margin<br>4: Portfolio margin |
| acctStpMode | String | Account self-trade prevention mode<br>cancel_maker<br>cancel_taker<br>cancel_both<br>The default value is cancel_maker. Users can log in to the webpage through the master account to modify this configuration |
| posMode | String | Position mode<br>long_short_mode: long/short, only applicable to FUTURES/SWAP<br>net_mode: net |
| autoLoan | Boolean | Whether to borrow coins automatically<br>true: borrow coins automatically<br>false: not borrow coins automatically |
| greeksType | String | Current display type of Greeks<br>PA: Greeks in coins<br>BS: Black-Scholes Greeks in dollars |
| level | String | The user level of the current real trading volume on the platform, e.g Lv1, which means regular user level. |
| levelTmp | String | Temporary experience user level of special users, e.g Lv1 |
| ctIsoMode | String | Contract isolated margin trading settings<br>automatic: Auto transfers<br>autonomy: Manual transfers |
| mgnIsoMode | String | Margin isolated margin trading settings<br>auto_transfers_ccy: New auto transfers, enabling both base and quote currency as the margin for isolated margin trading<br>automatic: Auto transfers<br>quick_margin: Quick Margin Mode (For new accounts, including subaccounts, some defaults will be automatic, and others will be quick_margin) |
| spotOffsetType | String | Risk offset type<br>1: Spot-Derivatives(USDT) to be offsetted<br>2: Spot-Derivatives(Coin) to be offsetted<br>3: Only derivatives to be offsetted<br>Only applicable to Portfolio margin<br>(Deprecated) |
| roleType | String | Role type<br>0: General user<br>1: Leading trader<br>2: Copy trader |
| traderInsts | Array of strings | Leading trade instruments, only applicable to Leading trader |
| spotRoleType | String | SPOT copy trading role type.<br>0: General user；1: Leading trader；2: Copy trader |
| spotTraderInsts | Array of strings | Spot lead trading instruments, only applicable to lead trader |
| opAuth | String | Whether the optional trading was activated<br>0: not activate<br>1: activated |
| kycLv | String | Main account KYC level<br>0: No verification<br>1: level 1 completed<br>2: level 2 completed<br>3: level 3 completed<br>If the request originates from a subaccount, kycLv is the KYC level of the main account.<br>If the request originates from the main account, kycLv is the KYC level of the current account. |
| label | String | API key note of current request API key. No more than 50 letters (case sensitive) or numbers, which can be pure letters or pure numbers. |
| ip | String | IP addresses that linked with current API key, separate with commas if more than one, e.g. 117.37.203.58,117.37.203.57. It is an empty string "" if there is no IP bonded. |
| perm | String | The permission of the current requesting API key or Access token<br>read_only: Read<br>trade: Trade<br>withdraw: Withdraw |
| liquidationGear | String | The maintenance margin ratio level of liquidation alert<br>3 and -1 means that you will get hourly liquidation alerts on app and channel "Position risk warning" when your margin level drops to or below 300%. -1 is the initial value which has the same effect as -3<br>0 means that there is not alert |
| enableSpotBorrow | Boolean | Whether borrow is allowed or not in Spot mode<br>true: Enabled<br>false: Disabled |
| spotBorrowAutoRepay | Boolean | Whether auto-repay is allowed or not in Spot mode<br>true: Enabled<br>false: Disabled |
| type | String | Account type<br>0: Main account<br>1: Standard sub-account<br>2: Managed trading sub-account<br>5: Custody trading sub-account - Copper<br>9: Managed trading sub-account - Copper<br>12: Custody trading sub-account - Komainu |


# Get maximum order quantity

The maximum quantity to buy or sell. It corresponds to the "sz" from placement.

**Rate Limit:** 20 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

## HTTP Request
```
GET /api/v5/account/max-size
```

## Request Example
```
GET /api/v5/account/max-size?instId=BTC-USDT&tdMode=cash
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Single instrument or multiple instruments (no more than 5) in the smae instrument type separated with comma, e.g. BTC-USDT,ETH-USDT |
| tdMode | String | Yes | Trade mode<br>cash |
| px | String | No | Price<br>The parameter will be ignored when multiple instruments are specified. |

## Response Example

```
{
    "code": "0",
    "msg": "",
    "data": [{
        "ccy": "BTC",
        "instId": "BTC-USDT",
        "maxBuy": "0.0500695098559788",
        "maxSell": "64.4798671570072269"
  }]
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instId | String | Instrument ID |
| ccy | String | Currency used for margin |
| maxBuy | String | SPOT: The maximum quantity in base currency that you can buy |
| maxSell | String | SPOT: The maximum quantity in quote currency that you can sell |

# Get maximum available balance/equity

**Rate Limit:** 20 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

## HTTP Request
```
GET /api/v5/account/max-avail-size
```

## Request Example

```
# Query maximum available transaction amount for SPOT BTC-USDT
GET /api/v5/account/max-avail-size?instId=BTC-USDT&tdMode=cash
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Single instrument or multiple instruments (no more than 5) separated with comma, e.g. BTC-USDT,ETH-USDT |
| tdMode | String | Yes | Trade mode<br>cash |

## Response Example

```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "instId": "BTC-USDT",
      "availBuy": "100",
      "availSell": "1"
    }
  ]
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instId | String | Instrument ID |
| availBuy | String | Amount available to buy |
| availSell | String | Amount available to sell |

💡 In the case of SPOT, availBuy is in the quote currency, and availSell is in the base currency.

# Get fee rates

**Rate Limit:** 5 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

## HTTP Request
```
GET /api/v5/account/trade-fee
```

## Request Example

```
# Query trade fee rate of SPOT BTC-USDT
GET /api/v5/account/trade-fee?instType=SPOT&instId=BTC-USDT
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instType | String | Yes | Instrument type<br>SPOT |
| instId | String | No | Instrument ID, e.g. BTC-USDT<br>Applicable to SPOT |

## Response Example

```
{
  "code": "0",
  "msg": "",
  "data": [{
    "category": "1", //Deprecated
    "delivery": "",
    "exercise": "",
    "instType": "SPOT",
    "level": "lv1",
    "maker": "-0.0008",
    "makerU": "",
    "makerUSDC": "",
    "taker": "-0.001",
    "takerU": "",
    "takerUSDC": "",
    "ts": "1608623351857",
    "fiat": []
   }
  ]
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| level | String | Fee rate Level |
| taker | String | For SPOT, it is taker fee rate of the USDT trading pairs. |
| maker | String | For SPOT, it is maker fee rate of the USDT trading pairs. |
| takerU | String | Taker fee rate of USDT-margined contracts, only applicable to FUTURES/SWAP |
| makerU | String | Maker fee rate of USDT-margined contracts, only applicable to FUTURES/SWAP |
| delivery | String | Delivery fee rate |
| exercise | String | Fee rate for exercising the option |
| instType | String | Instrument type |
| takerUSDC | String | For SPOT, it is taker fee rate of the USDⓈ&Crypto trading pairs. |
| makerUSDC | String | For SPOT, it is maker fee rate of the USDⓈ&Crypto trading pairs. |
| ts | String | Data return time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| category | String | Currency category. Note: this parameter is already deprecated |
| fiat | Array of objects | Details of fiat fee rate |
| > ccy | String | Fiat currency. |
| > taker | String | Taker fee rate |
| > maker | String | Maker fee rate |

💡 **Remarks:**  
The fee rate like maker and taker: positive number, which means the rate of rebate; negative number, which means the rate of commission.

💡 USDⓈ represent the stablecoin besides USDT and USDC

# Get maximum withdrawals

Retrieve the maximum transferable amount from trading account to funding account. If no currency is specified, the transferable amount of all owned currencies will be returned.

**Rate Limit:** 20 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

## HTTP Request
```
GET /api/v5/account/max-withdrawal
```

## Request Example

```
GET /api/v5/account/max-withdrawal
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | No | Single currency or multiple currencies (no more than 20) separated with comma, e.g. BTC or BTC,ETH. |

## Response Example

```
{
    "code": "0",
    "msg": "",
    "data": [{
            "ccy": "BTC",
            "maxWd": "124",
            "maxWdEx": "125",
            "spotOffsetMaxWd": "",
            "spotOffsetMaxWdEx": ""
        },
        {
            "ccy": "ETH",
            "maxWd": "10",
            "maxWdEx": "12",
            "spotOffsetMaxWd": "",
            "spotOffsetMaxWdEx": ""
        }
    ]
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency |
| maxWd | String | Max withdrawal (excluding borrowed assets under Spot mode/Multi-currency margin/Portfolio margin) |
| maxWdEx | String | Max withdrawal (including borrowed assets under Spot mode/Multi-currency margin/Portfolio margin) |
| spotOffsetMaxWd | String | Max withdrawal under Spot-Derivatives risk offset mode (excluding borrowed assets under Portfolio margin)<br>Applicable to Portfolio margin |
| spotOffsetMaxWdEx | String | Max withdrawal under Spot-Derivatives risk offset mode (including borrowed assets under Portfolio margin)<br>Applicable to Portfolio margin |

# WebSocket

## Account channel

Retrieve account information. Data will be pushed when triggered by events such as placing order, canceling order, transaction execution, etc. It will also be pushed in regular interval according to subscription granularity.

Concurrent connection to this channel will be restricted by the following rules: WebSocket connection count limit.

**URL Path**  
`/ws/v5/private` (required login)

## Request Example : single

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "account",
      "ccy": "BTC"
    }
  ]
}
```

## Request Example

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "account",
      "extraParams": "
        {
          \"updateInterval\": \"0\"
        }
      "
    }
  ]
}
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message<br>Provided by client. It will be returned in response message for identifying the corresponding request.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>subscribe<br>unsubscribe |
| args | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name<br>account |
| > ccy | String | No | Currency |
| > extraParams | String | No | Additional configuration |
| >> updateInterval | int | No | 0: only push due to account events<br>The data will be pushed both by events and regularly if this field is omitted or set to other values than 0.<br>The following format should be strictly obeyed when using this field.<br>"extraParams": "<br>{<br>\"updateInterval\": \"0\"<br>}<br>" |

## Successful Response Example : single

```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "account",
    "ccy": "BTC"
  },
  "connId": "a4d3ae55"
}
```

## Successful Response Example

```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "account"
  },
  "connId": "a4d3ae55"
}
```

## Failure Response Example

```
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"account\", \"ccy\" : \"BTC\"}]}",
  "connId": "a4d3ae55"
}
```

## Response parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message |
| event | String | Yes | Operation<br>subscribe<br>unsubscribe<br>error |
| arg | Object | No | Subscribed channel |
| > channel | String | Yes | Channel name<br>account |
| > ccy | String | No | Currency |
| code | String | No | Error code |
| msg | String | No | Error message |
| connId | String | Yes | WebSocket connection ID |

## Push Data Example

```
{
    "arg": {
        "channel": "account",
        "uid": "44*********584"
    },
    "eventType": "snapshot",
    "curPage": 1,
    "lastPage": true,
    "data": [{
        "adjEq": "55444.12216906034",
    "availEq": "55415.624719833286",
        "borrowFroz": "0",
        "details": [{
            "availBal": "4734.371190691436",
            "availEq": "4734.371190691435",
            "borrowFroz": "0",
            "cashBal": "4750.426970691436",
            "ccy": "USDT",
            "coinUsdPrice": "0.99927",
            "crossLiab": "0",
      "collateralEnabled": false,
      "collateralRestrict": false,
      "colBorrAutoConversion": "0",
            "disEq": "4889.379316336831",
            "eq": "4892.951170691435",
            "eqUsd": "4889.379316336831",
            "smtSyncEq": "0",
            "spotCopyTradingEq": "0",
            "fixedBal": "0",
            "frozenBal": "158.57998",
            "imr": "",
            "interest": "0",
            "isoEq": "0",
            "isoLiab": "0",
            "isoUpl": "0",
            "liab": "0",
            "maxLoan": "0",
            "mgnRatio": "",
            "mmr": "",
            "notionalLever": "",
            "ordFrozen": "0",
            "rewardBal": "0",
            "spotInUseAmt": "",
            "clSpotInUseAmt": "",
            "maxSpotInUseAmt": "",          
            "spotIsoBal": "0",
            "stgyEq": "150",
            "twap": "0",
            "uTime": "1705564213903",
            "upl": "-7.475800000000003",
            "uplLiab": "0",
            "spotBal": "",
            "openAvgPx": "",
            "accAvgPx": "",
            "spotUpl": "",
            "spotUplRatio": "",
            "totalPnl": "",
            "totalPnlRatio": ""
        }],
        "imr": "0",
        "isoEq": "0",
        "mgnRatio": "",
        "mmr": "0",
        "notionalUsd": "0",
        "notionalUsdForBorrow": "0",
        "notionalUsdForFutures": "0",
        "notionalUsdForOption": "0",
        "notionalUsdForSwap": "0",
        "ordFroz": "0",
        "totalEq": "",
        "uTime": "1705564223311",
        "upl": "0"
    }]
}
```

## Push data parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| arg | Object | Successfully subscribed channel |
| > channel | String | Channel name |
| > uid | String | User Identifier |
| eventType | String | Event type:<br>snapshot: Initial and regular snapshot push<br>event_update: Event-driven update push |
| curPage | Integer | Current page number.<br>Only applicable for snapshot events. Not included in event_update events. |
| lastPage | Boolean | Whether this is the last page of pagination:<br>true<br>false<br>Only applicable for snapshot events. Not included in event_update events. |
| data | Array of objects | Subscribed data |
| > uTime | String | The latest time to get account information, millisecond format of Unix timestamp, e.g. 1597026383085 |
| > totalEq | String | The total amount of equity in USD |
| > isoEq | String | Isolated margin equity in USD<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| > adjEq | String | Adjusted / Effective equity in USD<br>The net fiat value of the assets in the account that can provide margins for spot, expiry futures, perpetual futures and options under the cross-margin mode.<br>In multi-ccy or PM mode, the asset and margin requirement will all be converted to USD value to process the order check or liquidation.<br>Due to the volatility of each currency market, our platform calculates the actual USD value of each currency based on discount rates to balance market risks.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > availEq | String | Account level available equity, excluding currencies that are restricted due to the collateralized borrowing limit.<br>Applicable to Multi-currency margin/Portfolio margin |
| > ordFroz | String | Margin frozen for pending cross orders in USD<br>Only applicable to Spot mode/Multi-currency margin |
| > imr | String | Initial margin requirement in USD<br>The sum of initial margins of all open positions and pending orders under cross-margin mode in USD.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > mmr | String | Maintenance margin requirement in USD<br>The sum of maintenance margins of all open positions and pending orders under cross-margin mode in USD.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > borrowFroz | String | Potential borrowing IMR of the account in USD<br>Only applicable to Spot mode/Multi-currency margin/Portfolio margin. It is "" for other margin modes. |
| > mgnRatio | String | Maintenance margin ratio in USD.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > notionalUsd | String | Notional value of positions in USD<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > notionalUsdForBorrow | String | Notional value for Borrow in USD<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > notionalUsdForSwap | String | Notional value of positions for Perpetual Futures in USD<br>Applicable to Multi-currency margin/Portfolio margin |
| > notionalUsdForFutures | String | Notional value of positions for Expiry Futures in USD<br>Applicable to Multi-currency margin/Portfolio margin |
| > notionalUsdForOption | String | Notional value of positions for Option in USD<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| > upl | String | Cross-margin info of unrealized profit and loss at the account level in USD<br>Applicable to Multi-currency margin/Portfolio margin |
| > details | Array of objects | Detailed asset information in all currencies |
| >> ccy | String | Currency |
| >> eq | String | Equity of currency |
| >> cashBal | String | Cash Balance |
| >> uTime | String | Update time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| >> isoEq | String | Isolated margin equity of currency<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| >> availEq | String | Available equity of currency<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| >> disEq | String | Discount equity of currency in USD |
| >> fixedBal | String | Frozen balance for Dip Sniper and Peak Sniper |
| >> availBal | String | Available balance of currency |
| >> frozenBal | String | Frozen balance of currency |
| >> ordFrozen | String | Margin frozen for open orders<br>Applicable to Spot mode/Futures mode/Multi-currency margin |
| >> liab | String | Liabilities of currency<br>It is a positive value, e.g. 21625.64.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| >> upl | String | The sum of the unrealized profit & loss of all margin and derivatives positions of currency.<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| >> uplLiab | String | Liabilities due to Unrealized loss of currency<br>Applicable to Multi-currency margin/Portfolio margin |
| >> crossLiab | String | Cross Liabilities of currency<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| >> isoLiab | String | Isolated Liabilities of currency<br>Applicable to Multi-currency margin/Portfolio margin |
| >> rewardBal | String | Trial fund balance |
| >> mgnRatio | String | Cross Maintenance margin ratio of currency<br>The index for measuring the risk of a certain asset in the account.<br>Applicable to Futures mode and when there is cross position |
| >> imr | String | Cross initial margin requirement at the currency level<br>Applicable to Futures mode and when there is cross position |
| >> mmr | String | Cross maintenance margin requirement at the currency level<br>Applicable to Futures mode and when there is cross position |
| >> interest | String | Interest of currency<br>It is a positive value, e.g."9.01". Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| >> twap | String | System is forced repayment(TWAP) indicator<br>Divided into multiple levels from 0 to 5, the larger the number, the more likely the auto repayment will be triggered.<br>Applicable to Spot mode/Multi-currency margin/Portfolio margin |
| >> maxLoan | String | Max loan of currency<br>Applicable to cross of Spot mode/Multi-currency margin/Portfolio margin |
| >> eqUsd | String | Equity USD of currency |
| >> borrowFroz | String | Potential borrowing IMR of currency in USD<br>Only applicable to Spot mode/Multi-currency margin/Portfolio margin. It is "" for other margin modes. |
| >> notionalLever | String | Leverage of currency<br>Applicable to Futures mode |
| >> coinUsdPrice | String | Price index USD of currency |
| >> stgyEq | String | strategy equity |
| >> isoUpl | String | Isolated unrealized profit and loss of currency<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| >> spotInUseAmt | String | Spot in use amount<br>Applicable to Portfolio margin |
| >> clSpotInUseAmt | String | User-defined spot risk offset amount<br>Applicable to Portfolio margin |
| >> maxSpotInUseAmt | String | Max possible spot risk offset amount<br>Applicable to Portfolio margin |
| >> spotIsoBal | String | Spot isolated balance<br>Applicable to copy trading<br>Applicable to Spot mode/Futures mode |
| >> smtSyncEq | String | Smart sync equity<br>The default is "0", only applicable to copy trader. |
| >> spotCopyTradingEq | String | Spot smart sync equity.<br>The default is "0", only applicable to copy trader. |
| >> spotBal | String | Spot balance. The unit is currency, e.g. BTC. More details |
| >> openAvgPx | String | Spot average cost price. The unit is USD. More details |
| >> accAvgPx | String | Spot accumulated cost price. The unit is USD. More details |
| >> spotUpl | String | Spot unrealized profit and loss. The unit is USD. More details |
| >> spotUplRatio | String | Spot unrealized profit and loss ratio. More details |
| >> totalPnl | String | Spot accumulated profit and loss. The unit is USD. More details |
| >> totalPnlRatio | String | Spot accumulated profit and loss ratio. More details |
| >> collateralEnabled | Boolean | true: Collateral enabled<br>false: Collateral disabled<br>Applicable to Multi-currency margin |
| >> collateralRestrict | Boolean | Platform level collateralized borrow restriction<br>true<br>false |
| >> colBorrAutoConversion | String | Indicator of forced repayment when the collateralized borrowing on a crypto reaches the platform limit and users' trading accounts hold this crypto.<br>Divided into multiple levels from 1-5, the larger the number, the more likely the repayment will be triggered.<br>The default will be 0, indicating there is no risk currently. 5 means this user is undergoing auto conversion now.<br>Applicable to Spot mode/Futures mode/Multi-currency margin/Portfolio margin |

💡 "" will be returned for inapplicable fields under the current account level.

**Important notes:**
- The account data is sent on event basis and regular basis.
- The event push is not pushed in real-time. It is aggregated and pushed at a fixed time interval, around 50ms. For example, if multiple events occur within a fixed time interval, the system will aggregate them into a single message and push it at the end of the fixed time interval. If the data volume is too large, it may be split into multiple messages.
- The regular push sends updates regardless of whether there are activities in the trading account or not.
- Only currencies with non-zero balance will be pushed. Definition of non-zero balance: any value of eq, availEq, availBql parameters is not 0. If the data is too large to be sent in a single push message, it will be split into multiple messages.
- For example, when subscribing to account channel without specifying ccy and there are 5 currencies are with non-zero balance, all 5 currencies data will be pushed in initial snapshot and in regular update. Subsequently when there is change in balance or equity of an token, only the incremental data of that currency will be pushed triggered by this change.

# Order Book Trading

## Trade

All Trade API endpoints require authentication.

### POST / Place order

You can place an order only if you have sufficient funds.

**Rate Limit:** 60 requests per 2 seconds  
**Rate limit rule:** User ID + Instrument ID  
**Permission:** Trade

## HTTP Request
```
POST /api/v5/trade/order
```

## Request Example

```
# place order for SPOT
POST /api/v5/trade/order
body
{
    "instId":"BTC-USDT",
    "tdMode":"cash",
    "clOrdId":"b15",
    "side":"buy",
    "ordType":"limit",
    "px":"2.15",
    "sz":"2"
}
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Instrument ID, e.g. BTC-USDT |
| tdMode | String | Yes | Trade mode<br>cash |
| clOrdId | String | No | Client Order ID as assigned by the client<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag | String | No | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters. |
| side | String | Yes | Order side<br>buy<br>sell |
| ordType | String | Yes | Order type<br>market: Market order<br>limit: Limit order<br>post_only: Post-only order<br>fok: Fill-or-kill order<br>ioc: Immediate-or-cancel order |
| sz | String | Yes | Quantity to buy or sell |
| px | String | Conditional | Order price. Only applicable to limit,post_only,fok,iocorder. |
| tgtCcy | String | No | Whether the target currency uses the quote or base currency.<br>base_ccy: Base currency<br>quote_ccy: Quote currency<br>Only applicable to SPOT Market Orders<br>Default is quote_ccy for buy, base_ccy for sell |
| banAmend | Boolean | No | Whether to disallow the system from amending the size of the SPOT Market Order.<br>Valid options: true or false. The default value is false.<br>If true, system will not amend and reject the market order if user does not have sufficient funds.<br>Only applicable to SPOT Market Orders |
| stpId | String | No | Self trade prevention ID. Orders from the same master account with the same ID will be prevented from self trade.<br>Numerical integers defined by user in the range of 1<= x<= 999999999 (deprecated) |
| stpMode | String | No | Self trade prevention mode.<br>Default to cancel maker<br>cancel_maker,cancel_taker, cancel_both<br>Cancel both does not support FOK. |
| tradeQuoteCcy | String | No | The quote currency used for trading. Only applicable to SPOT.<br>The default value is the quote currency of the instId, for example: for BTC-USD, the default is USD. |
| attachAlgoOrds | Array of objects | No | TP/SL information attached when placing order |
| > attachAlgoClOrdId | String | No | Client-supplied Algo ID when placing order attaching TP/SL<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > tpTriggerPx | String | Conditional | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well. |
| > tpOrdPx | String | Conditional | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well.<br>If the price is -1, take-profit will be executed at the market price. |
| > slTriggerPx | String | Conditional | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price. |
| > slOrdPx | String | Conditional | Stop-loss order price<br>If you fill in this parameter, you should fill in the stop-loss trigger price.<br>If the price is -1, stop-loss will be executed at the market price. |
| > tpTriggerPxType | String | No | Take-profit trigger price type<br>last: last price<br>The default is last |
| > slTriggerPxType | String | No | Stop-loss trigger price type<br>last: last price<br>The default is last |
| > sz | String | Conditional | Size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs |
| > amendPxOnTriggerType | String | No | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. Whether slTriggerPx will move to avgPx when the first TP order is triggered<br>0: disable, the default value<br>1: Enable |

## Response Example

```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "312269865356374016",
      "tag": "",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| code | String | The result code, 0 means success |
| msg | String | The error message, empty if the code is 0 |
| data | Array of objects | Array of objects contains the response results |
| > ordId | String | Order ID |
| > clOrdId | String | Client Order ID as assigned by the client |
| > tag | String | Order tag |
| > sCode | String | The code of the event execution result, 0 means success. |
| > sMsg | String | Rejection or success message of event execution. |
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. 1597026383085123<br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. 1597026383085123 |

💡 **clOrdId**  
clOrdId is a user-defined unique ID used to identify the order. It will be included in the response parameters if you have specified during order submission, and can be used as a request parameter to the endpoints to query, cancel and amend orders.  
clOrdId must be unique among the clOrdIds of all pending orders.

💡 **ordType**  
Order type. When creating a new order, you must specify the order type. The order type you specify will affect: 1) what order parameters are required, and 2) how the matching system executes your order. The following are valid order types:
- **limit**: Limit order, which requires specified sz and px.
- **market**: Market order. For SPOT and MARGIN, market order will be filled with market price (by swiping opposite order book). For Expiry Futures and Perpetual Futures, market order will be placed to order book with most aggressive price allowed by Price Limit Mechanism. For OPTION, market order is not supported yet. As the filled price for market orders cannot be determined in advance, OKX reserves/freezes your quote currency by an additional 5% for risk check.
- **post_only**: Post-only order, which the order can only provide liquidity to the market and be a maker. If the order would have executed on placement, it will be canceled instead.
- **fok**: Fill or kill order. If the order cannot be fully filled, the order will be canceled. The order would not be partially filled.
- **ioc**: Immediate or cancel order. Immediately execute the transaction at the order price, cancel the remaining unfilled quantity of the order, and the order quantity will not be displayed in the order book.
- **optimal_limit_ioc**: Market order with ioc (immediate or cancel). Immediately execute the transaction of this market order, cancel the remaining unfilled quantity of the order, and the order quantity will not be displayed in the order book. Only applicable to Expiry Futures and Perpetual Futures.

💡 **sz**  
Quantity to buy or sell.
- For SPOT Buy and Sell Limit Orders, it refers to the quantity in base currency.
- For SPOT Buy Market Orders, it refers to the quantity in quote currency.
- For SPOT Sell Market Orders, it refers to the quantity in base currency.
- For SPOT Market Orders, it is set by tgtCcy.

💡 **tgtCcy**  
This parameter is used to specify the order quantity in the order request is denominated in the quantity of base or quote currency. This is applicable to SPOT Market Orders only.
- Base currency: base_ccy
- Quote currency: quote_ccy

If you use the Base Currency quantity for buy market orders or the Quote Currency for sell market orders, please note:
1. If the quantity you enter is greater than what you can buy or sell, the system will execute the order according to your maximum buyable or sellable quantity. If you want to trade according to the specified quantity, you should use Limit orders.
2. When the market price is too volatile, the locked balance may not be sufficient to buy the Base Currency quantity or sell to receive the Quote Currency that you specified. We will change the quantity of the order to execute the order based on best effort principle based on your account balance. In addition, we will try to over lock a fraction of your balance to avoid changing the order quantity.

2.1 Example of base currency buy market order:
Taking the market order to buy 10 LTCs as an example, and the user can buy 11 LTC. At this time, if 10 < 11, the order is accepted. When the LTC-USDT market price is 200, and the locked balance of the user is 3,000 USDT, as 200*10 < 3,000, the market order of 10 LTC is fully executed; If the market is too volatile and the LTC-USDT market price becomes 400, 400*10 > 3,000, the user's locked balance is not sufficient to buy using the specified amount of base currency, the user's maximum locked balance of 3,000 USDT will be used to settle the trade. Final transaction quantity becomes 3,000/400 = 7.5 LTC.

2.2 Example of quote currency sell market order:
Taking the market order to sell 1,000 USDT as an example, and the user can sell 1,200 USDT, 1,000 < 1,200, the order is accepted. When the LTC-USDT market price is 200, and the locked balance of the user is 6 LTC, as 1,000/200 < 6, the market order of 1,000 USDT is fully executed; If the market is too volatile and the LTC-USDT market price becomes 100, 100*6 < 1,000, the user's locked balance is not sufficient to sell using the specified amount of quote currency, the user's maximum locked balance of 6 LTC will be used to settle the trade. Final transaction quantity becomes 6 * 100 = 600 USDT.

💡 For placing order with TP/Sl, TP/SL algo order will be generated only when this order is filled fully, or there is no TP/SL algo order generated.

💡 **Mandatory self trade prevention (STP)**  
The trading platform imposes mandatory self trade prevention at master account level, which means the accounts under the same master account, including master account itself and all its affiliated sub-accounts, will be prevented from self trade. The default STP mode is `Cancel Maker`. Users can also utilize the stpMode request parameter of the placing order endpoint to determine the stpMode of a certain order.  
Mandatory self trade prevention will not lead to latency.

There are three STP modes. The STP mode is always taken based on the configuration in the taker order.
1. **Cancel Maker**: This is the default STP mode, which cancels the maker order to prevent self-trading. Then, the taker order continues to match with the next order based on the order book priority.
2. **Cancel Taker**: The taker order is canceled to prevent self-trading. If the user's own maker order is lower in the order book priority, the taker order is partially filled and then canceled. FOK orders are always honored and canceled if they would result in self-trading.
3. **Cancel Both**: Both taker and maker orders are canceled to prevent self-trading. If the user's own maker order is lower in the order book priority, the taker order is partially filled. Then, the remaining quantity of the taker order and the first maker order are canceled. FOK orders are not supported in this mode.

### POST / Place multiple orders

Place orders in batches. Maximum 20 orders can be placed per request.  
Request parameters should be passed in the form of an array. Orders will be placed in turn

**Rate Limit:** 300 orders per 2 seconds  
**Rate limit rule:** User ID + Instrument ID  
**Permission:** Trade

💡 Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Place order`.

## HTTP Request
```
POST /api/v5/trade/batch-orders
```

## Request Example

```
# batch place order for SPOT
POST /api/v5/trade/batch-orders
body
[
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b15",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    },
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b16",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    }
]
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Instrument ID, e.g. BTC-USDT |
| tdMode | String | Yes | Trade mode<br>cash |
| clOrdId | String | No | Client Order ID as assigned by the client<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag | String | No | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters. |
| side | String | Yes | Order side<br>buy<br>sell |
| ordType | String | Yes | Order type<br>market: Market order<br>limit: Limit order<br>post_only: Post-only order<br>fok: Fill-or-kill order<br>ioc: Immediate-or-cancel order |
| sz | String | Yes | Quantity to buy or sell |
| px | String | Conditional | Order price. Only applicable to limit,post_only,fok,iocorder. |
| tgtCcy | String | No | Whether the target currency uses the quote or base currency.<br>base_ccy: Base currency ,quote_ccy: Quote currency<br>Only applicable to SPOT Market Orders<br>Default is quote_ccy for buy, base_ccy for sell |
| banAmend | Boolean | No | Whether to disallow the system from amending the size of the SPOT Market Order.<br>Valid options: true or false. The default value is false.<br>If true, system will not amend and reject the market order if user does not have sufficient funds.<br>Only applicable to SPOT Market Orders |
| stpId | String | No | Self trade prevention ID. Orders from the same master account with the same ID will be prevented from self trade.<br>Numerical integers defined by user in the range of 1<= x<= 999999999 (deprecated) |
| stpMode | String | No | Self trade prevention mode.<br>Default to cancel maker<br>cancel_maker,cancel_taker, cancel_both<br>Cancel both does not support FOK. |
| tradeQuoteCcy | String | No | The quote currency used for trading. Only applicable to SPOT.<br>The default value is the quote currency of the instId, for example: for BTC-USD, the default is USD. |
| attachAlgoOrds | Array of objects | No | TP/SL information attached when placing order |
| > attachAlgoClOrdId | String | No | Client-supplied Algo ID when placing order attaching TP/SL<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > tpTriggerPx | String | Conditional | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well. |
| > tpOrdPx | String | Conditional | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well.<br>If the price is -1, take-profit will be executed at the market price. |
| > slTriggerPx | String | Conditional | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price. |
| > slOrdPx | String | Conditional | Stop-loss order price<br>If you fill in this parameter, you should fill in the stop-loss trigger price.<br>If the price is -1, stop-loss will be executed at the market price. |
| > tpTriggerPxType | String | No | Take-profit trigger price type<br>last: last price<br>The default is last |
| > slTriggerPxType | String | No | Stop-loss trigger price type<br>last: last price<br>The default is last |
| > sz | String | Conditional | Size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs |
| > amendPxOnTriggerType | String | No | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. Whether slTriggerPx will move to avgPx when the first TP order is triggered<br>0: disable, the default value<br>1: Enable |

## Response Example

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "tag":"",
            "sCode":"0",
            "sMsg":""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "tag":"",
            "sCode":"0",
            "sMsg":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| code | String | The result code, 0 means success |
| msg | String | The error message, empty if the code is 0 |
| data | Array of objects | Array of objects contains the response results |
| > ordId | String | Order ID |
| > clOrdId | String | Client Order ID as assigned by the client |
| > tag | String | Order tag |
| > sCode | String | The code of the event execution result, 0 means success. |
| > sMsg | String | Rejection or success message of event execution. |
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. 1597026383085123<br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. 1597026383085123 |

💡 **clOrdId**  
clOrdId is a user-defined unique ID used to identify the order. It will be included in the response parameters if you have specified during order submission, and can be used as a request parameter to the endpoints to query, cancel and amend orders.  
clOrdId must be unique among all pending orders and the current request.

### POST / Cancel order

Cancel an incomplete order.

**Rate Limit:** 60 requests per 2 seconds  
**Rate limit rule:** User ID + Instrument ID  
**Permission:** Trade

## HTTP Request
```
POST /api/v5/trade/cancel-order
```

## Request Example

```
POST /api/v5/trade/cancel-order
body
{
    "ordId":"590908157585625111",
    "instId":"BTC-USDT"
}
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Instrument ID, e.g. BTC-USDT |
| ordId | String | Conditional | Order ID<br>Either ordId or clOrdId is required. If both are passed, ordId will be used. |
| clOrdId | String | Conditional | Client Order ID as assigned by the client |

## Response Example

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "sCode":"0",
            "sMsg":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| code | String | The result code, 0 means success |
| msg | String | The error message, empty if the code is 0 |
| data | Array of objects | Array of objects contains the response results |
| > ordId | String | Order ID |
| > clOrdId | String | Client Order ID as assigned by the client |
| > sCode | String | The code of the event execution result, 0 means success. |
| > sMsg | String | Rejection message if the request is unsuccessful. |
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. 1597026383085123<br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. 1597026383085123 |

💡 Cancel order returns with sCode equal to 0. It is not strictly considered that the order has been canceled. It only means that your cancellation request has been accepted by the system server. The result of the cancellation is subject to the state pushed by the order channel or the get order state.

### POST / Cancel multiple orders

Cancel incomplete orders in batches. Maximum 20 orders can be canceled per request. Request parameters should be passed in the form of an array.

**Rate Limit:** 300 orders per 2 seconds  
**Rate limit rule:** User ID + Instrument ID  
**Permission:** Trade

💡 Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Cancel order`.

## HTTP Request
```
POST /api/v5/trade/cancel-batch-orders
```

## Request Example

```
POST /api/v5/trade/cancel-batch-orders
body
[
    {
        "instId":"BTC-USDT",
        "ordId":"590908157585625111"
    },
    {
        "instId":"BTC-USDT",
        "ordId":"590908544950571222"
    }
]
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Instrument ID, e.g. BTC-USDT |
| ordId | String | Conditional | Order ID<br>Either ordId or clOrdId is required. If both are passed, ordId will be used. |
| clOrdId | String | Conditional | Client Order ID as assigned by the client |

## Response Example

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "sCode":"0",
            "sMsg":""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "sCode":"0",
            "sMsg":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| code | String | The result code, 0 means success |
| msg | String | The error message, empty if the code is 0 |
| data | Array of objects | Array of objects contains the response results |
| > ordId | String | Order ID |
| > clOrdId | String | Client Order ID as assigned by the client |
| > sCode | String | The code of the event execution result, 0 means success. |
| > sMsg | String | Rejection message if the request is unsuccessful. |
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. 1597026383085123<br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. 1597026383085123 |

### POST / Amend order

Amend an incomplete order.

**Rate Limit:** 60 requests per 2 seconds  
**Rate limit rule:** User ID + Instrument ID  
**Permission:** Trade

## HTTP Request
```
POST /api/v5/trade/amend-order
```

## Request Example

```
POST /api/v5/trade/amend-order
body
{
    "ordId":"590909145319051111",
    "newSz":"2",
    "instId":"BTC-USDT"
}
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Instrument ID |
| cxlOnFail | Boolean | No | Whether the order needs to be automatically canceled when the order amendment fails<br>Valid options: false or true, the default is false. |
| ordId | String | Conditional | Order ID<br>Either ordId or clOrdId is required. If both are passed, ordId will be used. |
| clOrdId | String | Conditional | Client Order ID as assigned by the client |
| reqId | String | No | Client Request ID as assigned by the client for order amendment<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>The response will include the corresponding reqId to help you identify the request if you provide it in the request. |
| newSz | String | Conditional | New quantity after amendment. When amending a partially-filled order, the newSz should include the amount that has been filled. |
| newPx | String | Conditional | New price after amendment. |
| attachAlgoOrds | Array of objects | No | TP/SL information attached when placing order |
| > attachAlgoId | String | Conditional | The order ID of attached TP/SL order. It is required to identity the TP/SL order when amending. It will not be posted to algoId when placing TP/SL order after the general order is filled completely. |
| > attachAlgoClOrdId | String | Conditional | Client-supplied Algo ID when placing order attaching TP/SL<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > newTpTriggerPx | String | Conditional | Take-profit trigger price.<br>Either the take profit trigger price or order price is 0, it means that the take profit is deleted.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newTpOrdPx | String | Conditional | Take-profit order price<br>If the price is -1, take-profit will be executed at the market price.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newSlTriggerPx | String | Conditional | Stop-loss trigger price<br>Either the stop loss trigger price or order price is 0, it means that the stop loss is deleted.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newSlOrdPx | String | Conditional | Stop-loss order price<br>If the price is -1, stop-loss will be executed at the market price.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newTpTriggerPxType | String | Conditional | Take-profit trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>Only applicable to FUTURES/SWAP<br>If you want to add the take-profit, this parameter is required |
| > newSlTriggerPxType | String | Conditional | Stop-loss trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>Only applicable to FUTURES/SWAP<br>If you want to add the stop-loss, this parameter is required |
| > sz | String | Conditional | New size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs |
| > amendPxOnTriggerType | String | No | Whether to enable Cost-price SL. Only applicable to SL order of split TPs.<br>0: disable, the default value<br>1: Enable |

## Response Example

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
         "clOrdId":"",
         "ordId":"12344",
         "reqId":"b12344",
         "sCode":"0",
         "sMsg":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| code | String | The result code, 0 means success |
| msg | String | The error message, empty if the code is 0 |
| data | Array of objects | Array of objects contains the response results |
| > ordId | String | Order ID |
| > clOrdId | String | Client Order ID as assigned by the client |
| > reqId | String | Client Request ID as assigned by the client for order amendment. |
| > sCode | String | The code of the event execution result, 0 means success. |
| > sMsg | String | Rejection message if the request is unsuccessful. |
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. 1597026383085123<br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. 1597026383085123 |

💡 **newSz**  
If the new quantity of the order is less than or equal to the filled quantity when you are amending a partially-filled order, the order status will be changed to filled.

💡 The amend order returns sCode equal to 0. It is not strictly considered that the order has been amended. It only means that your amend order request has been accepted by the system server. The result of the amend is subject to the status pushed by the order channel or the order status query

### POST / Amend multiple orders

Amend incomplete orders in batches. Maximum 20 orders can be amended per request. Request parameters should be passed in the form of an array.

**Rate Limit:** 300 orders per 2 seconds  
**Rate limit rule:** User ID + Instrument ID  
**Permission:** Trade

Rate limit of this endpoint will also be affected by the rules Sub-account rate limit and Fill ratio based sub-account rate limit.

💡 Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Amend order`.

## HTTP Request
```
POST /api/v5/trade/amend-batch-orders
```

## Request Example

```
POST /api/v5/trade/amend-batch-orders
body
[
    {
        "ordId":"590909308792049444",
        "newSz":"2",
        "instId":"BTC-USDT"
    },
    {
        "ordId":"590909308792049555",
        "newSz":"2",
        "instId":"BTC-USDT"
    }
]
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Instrument ID |
| cxlOnFail | Boolean | No | Whether the order needs to be automatically canceled when the order amendment fails<br>false true, the default is false. |
| ordId | String | Conditional | Order ID<br>Either ordId or clOrdIdis required, if both are passed, ordId will be used. |
| clOrdId | String | Conditional | Client Order ID as assigned by the client |
| reqId | String | No | Client Request ID as assigned by the client for order amendment<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>The response will include the corresponding reqId to help you identify the request if you provide it in the request. |
| newSz | String | Conditional | New quantity after amendment. When amending a partially-filled order, the newSz should include the amount that has been filled. |
| newPx | String | Conditional | New price after amendment. |
| attachAlgoOrds | Array of objects | No | TP/SL information attached when placing order |
| > attachAlgoId | String | Conditional | The order ID of attached TP/SL order. It is required to identity the TP/SL order when amending. It will not be posted to algoId when placing TP/SL order after the general order is filled completely. |
| > attachAlgoClOrdId | String | Conditional | Client-supplied Algo ID when placing order attaching TP/SL<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > newTpTriggerPx | String | Conditional | Take-profit trigger price.<br>Either the take profit trigger price or order price is 0, it means that the take profit is deleted.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newTpOrdPx | String | Conditional | Take-profit order price<br>If the price is -1, take-profit will be executed at the market price.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newSlTriggerPx | String | Conditional | Stop-loss trigger price<br>Either the stop loss trigger price or order price is 0, it means that the stop loss is deleted.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newSlOrdPx | String | Conditional | Stop-loss order price<br>If the price is -1, stop-loss will be executed at the market price.<br>Only applicable to Expiry Futures and Perpetual Futures. |
| > newTpTriggerPxType | String | Conditional | Take-profit trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>Only applicable to FUTURES/SWAP<br>If you want to add the take-profit, this parameter is required |
| > newSlTriggerPxType | String | Conditional | Stop-loss trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>Only applicable to FUTURES/SWAP<br>If you want to add the stop-loss, this parameter is required |
| > sz | String | Conditional | New size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs |
| > amendPxOnTriggerType | String | No | Whether to enable Cost-price SL. Only applicable to SL order of split TPs.<br>0: disable, the default value<br>1: Enable |

## Response Example

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "reqId":"b12344",
            "sCode":"0",
            "sMsg":""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "reqId":"b12344",
            "sCode":"0",
            "sMsg":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| code | String | The result code, 0 means success |
| msg | String | The error message, empty if the code is 0 |
| data | Array of objects | Array of objects contains the response results |
| > ordId | String | Order ID |
| > clOrdId | String | Client Order ID as assigned by the client |
| > reqId | String | Client Request ID as assigned by the client for order amendment. |
| > sCode | String | The code of the event execution result, 0 means success. |
| > sMsg | String | Rejection message if the request is unsuccessful. |
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. 1597026383085123<br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. 1597026383085123 |

💡 **newSz**  
If the new quantity of the order is less than or equal to the filled quantity when you are amending a partially-filled order, the order status will be changed to filled.

### GET / Order details

Retrieve order details.

**Rate Limit:** 60 requests per 2 seconds  
**Rate limit rule (except Options):** User ID + Instrument ID  
**Rate limit rule (Options only):** User ID + Instrument Family  
**Permission:** Read

## HTTP Request
```
GET /api/v5/trade/order
```

## Request Example

```
GET /api/v5/trade/order?ordId=1753197687182819328&instId=BTC-USDT
```

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instId | String | Yes | Instrument ID, e.g. BTC-USDT<br>Only applicable to live instruments |
| ordId | String | Conditional | Order ID<br>Either ordId or clOrdId is required, if both are passed, ordId will be used |
| clOrdId | String | Conditional | Client Order ID as assigned by the client<br>If the clOrdId is associated with multiple orders, only the latest one will be returned. |

## Response Example

```
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0.00192834",
            "algoClOrdId": "",
            "algoId": "",
            "attachAlgoClOrdId": "",
            "attachAlgoOrds": [],
            "avgPx": "51858",
            "cTime": "1708587373361",
            "cancelSource": "",
            "cancelSourceReason": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "fee": "-0.00000192834",
            "feeCcy": "BTC",
            "fillPx": "51858",
            "fillSz": "0.00192834",
            "fillTime": "1708587373361",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "isTpLimit": "false",
            "lever": "",
            "linkedAlgoOrd": {
                "algoId": ""
            },
            "ordId": "680800019749904384",
            "ordType": "market",
            "pnl": "0",
            "posSide": "net",
            "px": "",
            "pxType": "",
            "pxUsd": "",
            "pxVol": "",
            "quickMgnType": "",
            "rebate": "0",
            "rebateCcy": "USDT",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "source": "",
            "state": "filled",
            "stpId": "",
            "stpMode": "",
            "sz": "100",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "quote_ccy",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "tradeId": "744876980",
            "tradeQuoteCcy": "USDT",
            "uTime": "1708587373362"
        }
    ],
    "msg": ""
}
```

## Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instType | String | Instrument type<br>SPOT<br>MARGIN<br>SWAP<br>FUTURES<br>OPTION |
| instId | String | Instrument ID |
| tgtCcy | String | Order quantity unit setting for sz<br>base_ccy: Base currency ,quote_ccy: Quote currency<br>Only applicable to SPOT Market Orders<br>Default is quote_ccy for buy, base_ccy for sell |
| ccy | String | Margin currency<br>Applicable to all isolated MARGIN orders and cross MARGIN orders in Futures mode. |
| ordId | String | Order ID |
| clOrdId | String | Client Order ID as assigned by the client |
| tag | String | Order tag |
| px | String | Price<br>For options, use coin as unit (e.g. BTC, ETH) |
| pxUsd | String | Options price in USD<br>Only applicable to options; return "" for other instrument types |
| pxVol | String | Implied volatility of the options order<br>Only applicable to options; return "" for other instrument types |
| pxType | String | Price type of options<br>px: Place an order based on price, in the unit of coin (the unit for the request parameter px is BTC or ETH)<br>pxVol: Place an order based on pxVol<br>pxUsd: Place an order based on pxUsd, in the unit of USD (the unit for the request parameter px is USD) |
| sz | String | Quantity to buy or sell |
| pnl | String | Profit and loss, Applicable to orders which have a trade and aim to close position. It always is 0 in other conditions |
| ordType | String | Order type<br>market: Market order<br>limit: Limit order<br>post_only: Post-only order<br>fok: Fill-or-kill order<br>ioc: Immediate-or-cancel order<br>optimal_limit_ioc: Market order with immediate-or-cancel order<br>mmp: Market Maker Protection (only applicable to Option in Portfolio Margin mode)<br>mmp_and_post_only: Market Maker Protection and Post-only order(only applicable to Option in Portfolio Margin mode)<br>op_fok: Simple options (fok) |
| side | String | Order side |
| posSide | String | Position side |
| tdMode | String | Trade mode |
| accFillSz | String | Accumulated fill quantity<br>The unit is base_ccy for SPOT and MARGIN, e.g. BTC-USDT, the unit is BTC; For market orders, the unit both is base_ccy when the tgtCcy is base_ccy or quote_ccy;<br>The unit is contract for FUTURES/SWAP/OPTION |
| fillPx | String | Last filled price. If none is filled, it will return "". |
| tradeId | String | Last traded ID |
| fillSz | String | Last filled quantity<br>The unit is base_ccy for SPOT and MARGIN, e.g. BTC-USDT, the unit is BTC; For market orders, the unit both is base_ccy when the tgtCcy is base_ccy or quote_ccy;<br>The unit is contract for FUTURES/SWAP/OPTION |
| fillTime | String | Last filled time |
| avgPx | String | Average filled price. If none is filled, it will return "". |
| state | String | State<br>canceled<br>live<br>partially_filled<br>filled<br>mmp_canceled |
| stpId | String | Self trade prevention ID<br>Return "" if self trade prevention is not applicable (deprecated) |
| stpMode | String | Self trade prevention mode |
| lever | String | Leverage, from 0.01 to 125.<br>Only applicable to MARGIN/FUTURES/SWAP |
| attachAlgoClOrdId | String | Client-supplied Algo ID when placing order attaching TP/SL. |
| tpTriggerPx | String | Take-profit trigger price. |
| tpTriggerPxType | String | Take-profit trigger price type.<br>last: last price<br>index: index price<br>mark: mark price |
| tpOrdPx | String | Take-profit order price. |
| slTriggerPx | String | Stop-loss trigger price. |
| slTriggerPxType | String | Stop-loss trigger price type.<br>last: last price<br>index: index price<br>mark: mark price |
| slOrdPx | String | Stop-loss order price. |
| attachAlgoOrds | Array of objects | TP/SL information attached when placing order |
| > attachAlgoId | String | The order ID of attached TP/SL order. It can be used to identity the TP/SL order when amending. It will not be posted to algoId when placing TP/SL order after the general order is filled completely. |
| > attachAlgoClOrdId | String | Client-supplied Algo ID when placing order attaching TP/SL<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > tpOrdKind | String | TP order kind<br>condition<br>limit |
| > tpTriggerPx | String | Take-profit trigger price. |
| > tpTriggerPxType | String | Take-profit trigger price type.<br>last: last price<br>index: index price<br>mark: mark price |
| > tpOrdPx | String | Take-profit order price. |
| > slTriggerPx | String | Stop-loss trigger price. |
| > slTriggerPxType | String | Stop-loss trigger price type.<br>last: last price<br>index: index price<br>mark: mark price |
| > slOrdPx | String | Stop-loss order price. |
| > sz | String | Size. Only applicable to TP order of split TPs |
| > amendPxOnTriggerType | String | Whether to enable Cost-price SL. Only applicable to SL order of split TPs.<br>0: disable, the default value<br>1: Enable |
| > failCode | String | The error code when failing to place TP/SL order, e.g. 51020<br>The default is "" |
| > failReason | String | The error reason when failing to place TP/SL order.<br>The default is "" |
| linkedAlgoOrd | Object | Linked SL order detail, only applicable to the order that is placed by one-cancels-the-other (OCO) order that contains the TP limit order. |
| > algoId | String | Algo ID |
| feeCcy | String | Fee currency |
| fee | String | Fee and rebate<br>For spot and margin, it is accumulated fee charged by the platform. It is always negative, e.g. -0.01.<br>For Expiry Futures, Perpetual Futures and Options, it is accumulated fee and rebate |
| rebateCcy | String | Rebate currency |
| source | String | Order source<br>6: The normal order triggered by the trigger order<br>7:The normal order triggered by the TP/SL order<br>13: The normal order triggered by the algo order<br>25:The normal order triggered by the trailing stop order<br>34: The normal order triggered by the chase order |
| rebate | String | Rebate amount, only applicable to spot and margin, the reward of placing orders from the platform (rebate) given to user who has reached the specified trading level. If there is no rebate, this field is "". |
| category | String | Category<br>normal<br>twap<br>adl<br>full_liquidation<br>partial_liquidation<br>delivery<br>ddh: Delta dynamic hedge |
| reduceOnly | String | Whether the order can only reduce the position size. Valid options: true or false. |
| isTpLimit | String | Whether it is TP limit order. true or false |
| cancelSource | String | Code of the cancellation source. |
| cancelSourceReason | String | Reason for the cancellation. |
| quickMgnType | String | Quick Margin type, Only applicable to Quick Margin Mode of isolated margin<br>manual, auto_borrow, auto_repay |
| algoClOrdId | String | Client-supplied Algo ID. There will be a value when algo order attaching algoClOrdId is triggered, or it will be "". |
| algoId | String | Algo ID. There will be a value when algo order is triggered, or it will be "". |
| uTime | String | Update time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| cTime | String | Creation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| tradeQuoteCcy | String | The quote currency used for trading. |


# GET / Order List

Retrieve all incomplete orders under the current account.  
Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

## HTTP Request  
`GET /api/v5/trade/orders-pending`

| Parameter  | Type    | Required | Description                              |
|------------|---------|----------|------------------------------------------|
| uly        | String  | No       | Underlying, e.g. BTC-USD (only applicable to MARGIN/FUTURES/SWAP/OPTION) |
| instFamily | String  | No       | Instrument family, e.g. BTC-USD (only applicable to MARGIN/FUTURES/SWAP/OPTION) |
| baseCcy    | String  | No       | Base currency, e.g. BTC in BTC-USDT (only applicable to SPOT/MARGIN) |
| quoteCcy   | String  | No       | Quote currency, e.g. USDT in BTC-USDT (only applicable to SPOT/MARGIN) |
| settleCcy  | String  | No       | Settlement and margin currency, e.g. BTC (only applicable to FUTURES/SWAP/OPTION) |
| ctVal      | String  | No       | Contract value (only applicable to FUTURES/SWAP/OPTION) |
| ctMult     | String  | No       | Contract multiplier (only applicable to FUTURES/SWAP/OPTION) |
| ctValCcy   | String  | No       | Contract value currency (only applicable to FUTURES/SWAP/OPTION) |
| optType    | String  | No       | Option type, C: Call, P: put (only applicable to OPTION) |
| stk        | String  | No       | Strike price |

### Request Example  
`GET /api/v5/trade/orders-pending?ordType=post_only,fok,ioc&instType=SPOT`

### Request Parameters

| Parameter | Type   | Required | Description                                                          |
|-----------|--------|----------|----------------------------------------------------------------------|
| instType  | String | No       | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION                 |
| uly       | String | No       | Underlying (Applicable to FUTURES/SWAP/OPTION)                       |
| instFamily| String | No       | Instrument family (Applicable to FUTURES/SWAP/OPTION)                |
| instId    | String | No       | Instrument ID, e.g. BTC-USD-200927                                  |
| ordType   | String | No       | Order type: market, limit, post_only, fok, ioc, optimal_limit_ioc, mmp, mmp_and_post_only, op_fok |
| state     | String | No       | State: live, partially_filled                                        |
| after     | String | No       | Pagination of data to return records earlier than the requested ordId |
| before    | String | No       | Pagination of data to return records newer than the requested ordId  |
| limit     | String | No       | Number of results per request. Max 100; default 100                  |

### Response Example
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0",
            "algoClOrdId": "",
            "algoId": "",
            "attachAlgoClOrdId": "",
            "attachAlgoOrds": [],
            "avgPx": "",
            "cTime": "1724733617998",
            "cancelSource": "",
            "cancelSourceReason": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "fee": "0",
            "feeCcy": "BTC",
            "fillPx": "",
            "fillSz": "0",
            "fillTime": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "isTpLimit": "false",
            "lever": "",
            "linkedAlgoOrd": {
                "algoId": ""
            },
            "ordId": "1752588852617379840",
            "ordType": "post_only",
            "pnl": "0",
            "posSide": "net",
            "px": "13013.5",
            "pxType": "",
            "pxUsd": "",
            "pxVol": "",
            "quickMgnType": "",
            "rebate": "0",
            "rebateCcy": "USDT",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "source": "",
            "state": "live",
            "stpId": "",
            "stpMode": "cancel_maker",
            "sz": "0.001",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "tradeId": "",
            "tradeQuoteCcy": "USDT",
            "uTime": "1724733617998"
        }
    ],
    "msg": ""
}

### Response Parameters

| Parameter      | Type    | Description                                                                                          |
|----------------|---------|-----------------------------------------------------------------------------------------------------|
| instType       | String  | Instrument type                                                                                     |
| instId         | String  | Instrument ID                                                                                       |
| tgtCcy         | String  | Order quantity unit setting for sz: base_ccy or quote_ccy (Applicable to SPOT Market Orders)       |
| ccy            | String  | Margin currency (Applicable to all isolated MARGIN orders and cross MARGIN orders in Futures mode)  |
| ordId          | String  | Order ID                                                                                           |
| clOrdId        | String  | Client Order ID as assigned by the client                                                         |
| tag            | String  | Order tag                                                                                         |
| px             | String  | Price (For options, use coin as unit e.g. BTC, ETH)                                               |
| pxUsd          | String  | Options price in USD (Only applicable to options; return "" for other instrument types)           |
| pxVol          | String  | Implied volatility of the options order (Only applicable to options; return "" for other types)   |
| pxType         | String  | Price type of options: px, pxVol, pxUsd                                                           |
| sz             | String  | Quantity to buy or sell                                                                            |
| pnl            | String  | Profit and loss, 0 if conditions don’t apply                                                     |
| ordType        | String  | Order type                                                                                        |
| side           | String  | Order side                                                                                        |
| posSide        | String  | Position side                                                                                    |
| tdMode         | String  | Trade mode                                                                                       |
| accFillSz      | String  | Accumulated fill quantity                                                                        |
| fillPx         | String  | Last filled price                                                                                |
| tradeId        | String  | Last trade ID                                                                                   |
| fillSz         | String  | Last filled quantity                                                                            |
| fillTime       | String  | Last filled time                                                                               |
| avgPx          | String  | Average filled price                                                                           |
| state          | String  | State: live, partially_filled                                                                 |
| lever          | String  | Leverage (only applicable to MARGIN/FUTURES/SWAP)                                             |
| attachAlgoClOrdId | String | Client-supplied Algo ID when placing order attaching TP/SL                                   |
| tpTriggerPx    | String  | Take-profit trigger price                                                                      |
| tpTriggerPxType| String  | Take-profit trigger price type: last, index, mark                                             |
| tpOrdPx       | String   | Take-profit order price                                                                       |
| slTriggerPx    | String  | Stop-loss trigger price                                                                        |
| slTriggerPxType| String  | Stop-loss trigger price type: last, index, mark                                              |
| slOrdPx       | String   | Stop-loss order price                                                                         |
| attachAlgoOrds | Array    | TP/SL information attached when placing order (see subfields)                                |
| linkedAlgoOrd  | Object   | Linked SL order detail (for OCO orders with TP limit order)                                   |
| stpId         | String   | Self trade prevention ID (deprecated)                                                         |
| stpMode       | String   | Self trade prevention mode                                                                    |
| feeCcy        | String   | Fee currency                                                                                  |
| fee           | String   | Fee and rebate                                                                               |
| rebateCcy     | String   | Rebate currency                                                                             |
| source        | String   | Order source codes: 6, 7, 13, 25, 34                                                         |
| rebate        | String   | Rebate amount (spot and margin only)                                                        |
| category      | String   | Category: normal                                                                            |
| reduceOnly    | String   | Whether order can only reduce position size (true/false)                                    |
| quickMgnType  | String   | Quick Margin type (only for Quick Margin Mode)                                            |
| algoClOrdId   | String   | Client-supplied Algo ID (if any)                                                          |
| algoId        | String   | Algo ID (if any)                                                                          |
| isTpLimit     | String   | Whether it is TP limit order (true/false)                                                |
| uTime         | String   | Update time in Unix timestamp (ms)                                                     |
| cTime         | String   | Creation time in Unix timestamp (ms)                                                   |
| cancelSource  | String   | Code of cancellation source                                                           |
| cancelSourceReason | String | Reason for cancellation                                                               |
| tradeQuoteCcy | String   | Quote currency used for trading                                                       |

***

# GET / Order history (last 7 days)

Get completed orders which are placed in the last 7 days, including those placed 7 days ago but completed in the last 7 days.  
The incomplete orders that have been canceled are only reserved for 2 hours.  
Rate Limit: 40 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

## HTTP Request  
`GET /api/v5/trade/orders-history`

### Request Example  
`GET /api/v5/trade/orders-history?ordType=post_only,fok,ioc&instType=SPOT`

### Request Parameters

| Parameter | Type    | Required | Description                          |
|-----------|---------|----------|--------------------------------------|
| instType  | String  | yes      | Instrument type (SPOT, MARGIN, SWAP, FUTURES, OPTION) |
| uly       | String  | No       | Underlying (Applicable to FUTURES/SWAP/OPTION)        |
| instFamily| String  | No       | Instrument family (Applicable to FUTURES/SWAP/OPTION) |
| instId    | String  | No       | Instrument ID, e.g. BTC-USDT                            |
| ordType   | String  | No       | Order type (market, limit, post_only, fok, ioc, etc.)  |
| state     | String  | No       | State (canceled, filled, mmp_canceled)                 |
| category  | String  | No       | Category (twap, adl, full_liquidation, etc.)           |
| after     | String  | No       | Pagination for records earlier than given ordId        |
| before    | String  | No       | Pagination for records newer than given ordId          |
| begin     | String  | No       | Begin timestamp cTime (ms)                             |
| end       | String  | No       | End timestamp cTime (ms)                               |
| limit     | String  | No       | Number of results, max 100, default 100                |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0.00192834",
            "algoClOrdId": "",
            "algoId": "",
            "attachAlgoClOrdId": "",
            "attachAlgoOrds": [],
            "avgPx": "51858",
            "cTime": "1708587373361",
            "cancelSource": "",
            "cancelSourceReason": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "fee": "-0.00000192834",
            "feeCcy": "BTC",
            "fillPx": "51858",
            "fillSz": "0.00192834",
            "fillTime": "1708587373361",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "lever": "",
            "linkedAlgoOrd": {
                "algoId": ""
            },
            "ordId": "680800019749904384",
            "ordType": "market",
            "pnl": "0",
            "posSide": "",
            "px": "",
            "pxType": "",
            "pxUsd": "",
            "pxVol": "",
            "quickMgnType": "",
            "rebate": "0",
            "rebateCcy": "USDT",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "source": "",
            "state": "filled",
            "stpId": "",
            "stpMode": "",
            "sz": "100",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "quote_ccy",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "tradeId": "744876980",
            "tradeQuoteCcy": "USDT",
            "uTime": "1708587373362",
            "isTpLimit": "false"
        }
    ],
    "msg": ""
}
```

### Response Parameters  
*(Same as in "GET / Order List" response parameters except `state` field now includes: canceled, filled, mmp_canceled.)*


# GET / Order history (last 3 months)

Get completed orders placed in the last 3 months, including those placed 3 months ago but completed in this period.  
Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

## HTTP Request  
`GET /api/v5/trade/orders-history-archive`

### Request Example  
`GET /api/v5/trade/orders-history-archive?ordType=post_only,fok,ioc&instType=SPOT`

### Request Parameters  
*(Same as "GET / Order history (last 7 days)" parameters)*

### Response Example  
*(Same structure and example as "GET / Order history (last 7 days)" response example)*


# GET / Transaction details (last 3 days)

Retrieve recently-filled transaction details in the last 3 days.  
Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

## HTTP Request  
`GET /api/v5/trade/fills`

### Request Example  
`GET /api/v5/trade/fills`

### Request Parameters

| Parameter | Type   | Required | Description                                          |
|-----------|--------|----------|------------------------------------------------------|
| instType  | String | No       | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION|
| uly       | String | No       | Underlying (Applicable to FUTURES/SWAP/OPTION)      |
| instFamily| String | No       | Instrument family (Applicable to FUTURES/SWAP/OPTION)|
| instId    | String | No       | Instrument ID, e.g. BTC-USDT                         |
| ordId     | String | No       | Order ID                                             |
| subType   | String | No       | Transaction type (codes as listed)                   |
| after     | String | No       | Pagination of data to return records earlier than given billId |
| before    | String | No       | Pagination of data to return records newer than given billId  |
| begin     | String | No       | Filter with begin timestamp ts (ms)                  |
| end       | String | No       | Filter with end timestamp ts (ms)                     |
| limit     | String | No       | Number of results per request (max 100, default 100)  |

### SubType Codes (Transaction Types)  

| Code | Meaning                                |
|-------|--------------------------------------|
| 1     | Buy                                  |
| 2     | Sell                                 |
| 3     | Open long                           |
| 4     | Open short                          |
| 5     | Close long                         |
| 6     | Close short                        |
| 100   | Partial liquidation close long      |
| 101   | Partial liquidation close short     |
| 102   | Partial liquidation buy             |
| 103   | Partial liquidation sell            |
| 104   | Liquidation long                   |
| 105   | Liquidation short                  |
| 106   | Liquidation buy                   |
| 107   | Liquidation sell                  |
| 110   | Liquidation transfer in            |
| 111   | Liquidation transfer out           |
| 118   | System token conversion transfer in|
| 119   | System token conversion transfer out|
| 112   | Delivery long                     |
| 113   | Delivery short                    |
| 125   | ADL close long                   |
| 126   | ADL close short                  |
| 127   | ADL buy                        |
| 128   | ADL sell                       |
| 212   | Auto borrow of quick margin       |
| 213   | Auto repay of quick margin        |
| 204   | Block trade buy                  |
| 205   | Block trade sell                 |
| 206   | Block trade open long            |
| 207   | Block trade open short           |
| 208   | Block trade close long           |
| 209   | Block trade close short          |
| 236   | Easy convert in                 |
| 237   | Easy convert out                |
| 270   | Spread trading buy              |
| 271   | Spread trading sell             |
| 272   | Spread trading open long        |
| 273   | Spread trading open short       |
| 274   | Spread trading close long       |
| 275   | Spread trading close short      |
| 324   | Move position buy               |
| 325   | Move position sell              |
| 326   | Move position open long         |
| 327   | Move position open short        |
| 328   | Move position close long        |
| 329   | Move position close short       |
| 376   | Collateralized borrowing auto conversion buy  |
| 377   | Collateralized borrowing auto conversion sell |

### Response Example
{
    "code": "0",
    "data": [
        {
            "side": "buy",
            "fillSz": "0.00192834",
            "fillPx": "51858",
            "fillPxVol": "",
            "fillFwdPx": "",
            "fee": "-0.00000192834",
            "fillPnl": "0",
            "ordId": "680800019749904384",
            "feeRate": "-0.001",
            "instType": "SPOT",
            "fillPxUsd": "",
            "instId": "BTC-USDT",
            "clOrdId": "",
            "posSide": "net",
            "billId": "680800019754098688",
            "subType": "1",
            "fillMarkVol": "",
            "tag": "",
            "fillTime": "1708587373361",
            "execType": "T",
            "fillIdxPx": "",
            "tradeId": "744876980",
            "fillMarkPx": "",
            "feeCcy": "BTC",
            "ts": "1708587373362"
        }
    ],
    "msg": ""
}

### Response Parameters

| Parameter       | Type   | Description                                                    |
|-----------------|--------|----------------------------------------------------------------|
| instType        | String | Instrument type                                               |
| instId          | String | Instrument ID                                                 |
| tradeId         | String | Last trade ID                                                |
| ordId           | String | Order ID ("" for block trading)                             |
| clOrdId         | String | Client-supplied order ID ("" for block trading)             |
| billId          | String | Bill ID                                                      |
| subType         | String | Transaction type code                                        |
| tag             | String | Order tag                                                   |
| fillPx          | String | Last filled price (same as px from "Get bills details")     |
| fillSz          | String | Last filled quantity                                        |
| fillIdxPx       | String | Index price at trade execution time                        |
| fillPnl         | String | Last filled PnL (applicable to closing orders)             |
| fillPxVol       | String | Implied volatility when filled (Only for options)          |
| fillPxUsd       | String | Options price when filled (Only for options)                |
| fillMarkVol     | String | Mark volatility when filled (Only for options)              |
| fillFwdPx       | String | Forward price when filled (Only for options)                |
| fillMarkPx      | String | Mark price when filled (Applicable to FUTURES, SWAP, OPTION) |
| side            | String | Order side: buy/sell                                        |
| posSide         | String | Position side: long/short/net                              |
| execType        | String | Liquidity taker or maker (T: taker, M: maker)               |
| feeCcy          | String | Trading fee or rebate currency                              |
| fee             | String | Amount of fee or rebate                                    |
| ts              | String | Data generation time (Unix timestamp ms)                   |
| fillTime        | String | Trade time (same as fillTime for order channel)             |
| feeRate         | String | Fee rate (for SPOT and MARGIN only)                        |


# GET / Transaction details (last 1 year)

This endpoint can retrieve data from the last 1 year since July 1, 2024.

Rate Limit: 10 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

## HTTP Request  
`GET /api/v5/trade/fills-history`

### Request Example  

`GET /api/v5/trade/fills-history?instType=SPOT`

### Request Parameters

| Parameter  | Type   | Required | Description                                      |
|------------|--------|----------|--------------------------------------------------|
| instType   | String | YES      | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION |
| uly        | String | No       | Underlying (Applicable to FUTURES/SWAP/OPTION)   |
| instFamily | String | No       | Instrument family (Applicable to FUTURES/SWAP/OPTION) |
| instId     | String | No       | Instrument ID, e.g. BTC-USDT                     |
| ordId      | String | No       | Order ID                                         |
| subType    | String | No       | Transaction type (see list below)                |
| after      | String | No       | Pagination for records earlier than requested billId |
| before     | String | No       | Pagination for records newer than requested billId   |
| begin      | String | No       | Filter with a begin timestamp (ms)               |
| end        | String | No       | Filter with an end timestamp (ms)                |
| limit      | String | No       | Number of results. Max 100. Default 100          |

#### subType Transaction Types

| Code | Description                               |
|------|-------------------------------------------|
| 1    | Buy                                      |
| 2    | Sell                                     |
| 3    | Open long                                |
| 4    | Open short                               |
| 5    | Close long                               |
| 6    | Close short                              |
| 100  | Partial liquidation close long           |
| 101  | Partial liquidation close short          |
| 102  | Partial liquidation buy                  |
| 103  | Partial liquidation sell                 |
| 104  | Liquidation long                         |
| 105  | Liquidation short                        |
| 106  | Liquidation buy                          |
| 107  | Liquidation sell                         |
| 110  | Liquidation transfer in                  |
| 111  | Liquidation transfer out                 |
| 118  | System token conversion transfer in      |
| 119  | System token conversion transfer out     |
| 112  | Delivery long                            |
| 113  | Delivery short                           |
| 125  | ADL close long                           |
| 126  | ADL close short                          |
| 127  | ADL buy                                  |
| 128  | ADL sell                                 |
| 212  | Auto borrow of quick margin              |
| 213  | Auto repay of quick margin               |
| 204  | Block trade buy                          |
| 205  | Block trade sell                         |
| 206  | Block trade open long                    |
| 207  | Block trade open short                   |
| 208  | Block trade close long                   |
| 209  | Block trade close short                  |
| 236  | Easy convert in                          |
| 237  | Easy convert out                         |
| 270  | Spread trading buy                       |
| 271  | Spread trading sell                      |
| 272  | Spread trading open long                 |
| 273  | Spread trading open short                |
| 274  | Spread trading close long                |
| 275  | Spread trading close short               |
| 324  | Move position buy                        |
| 325  | Move position sell                       |
| 326  | Move position open long                  |
| 327  | Move position open short                 |
| 328  | Move position close long                 |
| 329  | Move position close short                |
| 376  | Collateralized borrowing auto conversion buy |
| 377  | Collateralized borrowing auto conversion sell |

### Response Example  

```
{
    "code": "0",
    "data": [
        {
            "side": "buy",
            "fillSz": "0.00192834",
            "fillPx": "51858",
            "fillPxVol": "",
            "fillFwdPx": "",
            "fee": "-0.00000192834",
            "fillPnl": "0",
            "ordId": "680800019749904384",
            "feeRate": "-0.001",
            "instType": "SPOT",
            "fillPxUsd": "",
            "instId": "BTC-USDT",
            "clOrdId": "",
            "posSide": "net",
            "billId": "680800019754098688",
            "subType": "1",
            "fillMarkVol": "",
            "tag": "",
            "fillTime": "1708587373361",
            "execType": "T",
            "fillIdxPx": "",
            "tradeId": "744876980",
            "fillMarkPx": "",
            "feeCcy": "BTC",
            "ts": "1708587373362"
        }
    ],
    "msg": ""
}
```

### Response Parameters

| Parameter    | Type   | Description     |
|--------------|--------|-----------------|
| instType     | String | Instrument type |
| instId       | String | Instrument ID   |
| tradeId      | String | Last trade ID   |
| ordId        | String | Order ID        |
| clOrdId      | String | Client Order ID as assigned by the client |
| billId       | String | Bill ID         |
| subType      | String | Transaction type|
| tag          | String | Order tag       |
| fillPx       | String | Last filled price|
| fillSz       | String | Last filled quantity|
| fillIdxPx    | String | Index price at the moment of trade execution|
| fillPnl      | String | Last filled profit and loss|
| fillPxVol    | String | Implied volatility when filled|
| fillPxUsd    | String | Options price when filled, in the unit of USD|
| fillMarkVol  | String | Mark volatility when filled|
| fillFwdPx    | String | Forward price when filled|
| fillMarkPx   | String | Mark price when filled|
| side         | String | Order side      |
| posSide      | String | Position side   |
| execType     | String | Liquidity taker or maker|
| feeCcy       | String | Trading fee or rebate currency|
| fee          | String | The amount of trading fee or rebate|
| ts           | String | Data generation time (milliseconds)|
| fillTime     | String | Trade time      |
| feeRate      | String | Fee rate (SPOT/MARGIN only)|


# POST / Cancel All After

Cancel all pending orders after the countdown timeout. Applicable to all trading symbols through order book (except Spread trading)... Rate Limit: 1 request per second  
Rate limit rule: User ID + tag  
Permission: Trade  

## HTTP Request  

`POST /api/v5/trade/cancel-all-after`

### Request Example  
```
POST /api/v5/trade/cancel-all-after
{
   "timeOut":"60"
}
```

### Request Parameters

| Parameter | Type   | Required | Description                                          |
|-----------|--------|----------|------------------------------------------------------|
| timeOut   | String | Yes      | The countdown for order cancellation, in seconds (Range: 0, ). Setting timeOut to 0 disables Cancel All After. |
| tag       | String | No       | CAA order tag (up to 16 characters)                  |

### Response Example  

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "triggerTime":"1587971460",
            "tag":"",
            "ts":"1587971400"
        }
    ]
}
```

### Response Parameters

| Parameter    | Type   | Description                          |
|--------------|--------|--------------------------------------|
| triggerTime  | String | The time the cancellation is triggered (0 means disabled)|
| tag          | String | CAA order tag                        |
| ts           | String | Time request is received             |

***

# GET / Account rate limit

Get account rate limit related information.

Only new order requests and amendment order requests will be counted towards this limit. For batch order requests consisting of multiple orders, each order will be counted individually.

Rate Limit: 1 request per second  
Rate limit rule: User ID  

## HTTP Request  

`GET /api/v5/trade/account-rate-limit`

### Request Example  
`GET /api/v5/trade/account-rate-limit`

### Request Parameters

_None_

### Response Example  

```
{
   "code":"0",
   "data":[
      {
         "accRateLimit":"2000",
         "fillRatio":"0.1234",
         "mainFillRatio":"0.1234",
         "nextAccRateLimit":"2000",
         "ts":"123456789000"
      }
   ],
   "msg":""
}
```

### Response Parameters

| Parameter        | Type   | Description                                                             |
|------------------|--------|-------------------------------------------------------------------------|
| fillRatio        | String | Sub account fill ratio during the monitoring period (VIP 5+)             |
| mainFillRatio    | String | Master account aggregated fill ratio during the monitoring period (VIP 5+)|
| accRateLimit     | String | Current sub-account rate limit per two seconds                           |
| nextAccRateLimit | String | Expected sub-account rate limit per two seconds in the next period (VIP 5+)|
| ts               | String | Data update time                                                         |

***

# WS / Order channel

Retrieve order information via WebSocket. Data will not be pushed when first subscribed. Data will only be pushed when there are order updates.

URL Path  
`/ws/v5/private` (required login)

### Request Example : Single

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "orders",
      "instType": "FUTURES",
      "instId": "BTC-USD-200329"
    }
  ]
}
```

### Request Example

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "orders",
      "instType": "FUTURES",
      "instFamily": "BTC-USD"
    }
  ]
}
```

### Request Parameters

| Parameter  | Type   | Required | Description                                        |
|------------|--------|----------|----------------------------------------------------|
| id         | String | No       | Unique identifier of the message                   |
| op         | String | Yes      | Operation: subscribe or unsubscribe                |
| args       | Array  | Yes      | List of subscribed channels                        |

args Structure:

| Parameter    | Type   | Required | Description                 |
|--------------|--------|----------|-----------------------------|
| channel      | String | Yes      | Channel name: orders        |
| instType     | String | Yes      | Instrument type: SPOT, MARGIN, SWAP, FUTURES, OPTION, ANY |
| instFamily   | String | No       | Instrument family (optional)|
| instId       | String | No       | Instrument ID (optional)    |


### Successful Response Example : Single

```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "orders",
    "instType": "FUTURES",
    "instId": "BTC-USD-200329"
  },
  "connId": "a4d3ae55"
}
```

### Failure Response Example

```
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"orders\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

### Response parameters

| Parameter   | Type   | Required | Description      |
|-------------|--------|----------|------------------|
| id          | String | No       | Unique identifier of the message|
| event       | String | Yes      | Event: subscribe, unsubscribe, error|
| arg         | Object | No       | Subscribed channel              |
| code        | String | No       | Error code                      |
| msg         | String | No       | Error message                   |
| connId      | String | Yes      | WebSocket connection ID         |

arg Structure:

| Parameter    | Type   | Required | Description      |
|--------------|--------|----------|------------------|
| channel      | String | Yes      | Channel name     |
| instType     | String | Yes      | Instrument type  |
| instFamily   | String | No       | Instrument family|
| instId       | String | No       | Instrument ID    |

***

### Push Data Example

```
{
    "arg": {
        "channel": "orders",
        "instType": "SPOT",
        "instId": "BTC-USDT",
        "uid": "614488474791936"
    },
    "data": [
        {
            "accFillSz": "0.001",
            "algoClOrdId": "",
            "algoId": "",
            "amendResult": "",
            "amendSource": "",
            "avgPx": "31527.1",
            "cancelSource": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "code": "0",
            "cTime": "1654084334977",
            "execType": "M",
            "fee": "-0.02522168",
            "feeCcy": "USDT",
            "fillFee": "-0.02522168",
            "fillFeeCcy": "USDT",
            "fillNotionalUsd": "31.50818374",
            "fillPx": "31527.1",
            "fillSz": "0.001",
            "fillPnl": "0.01",
            "fillTime": "1654084353263",
            "fillPxVol": "",
            "fillPxUsd": "",
            "fillMarkVol": "",
            "fillFwdPx": "",
            "fillMarkPx": "",
            "fillIdxPx": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "lever": "0",
            "msg": "",
            "notionalUsd": "31.50818374",
            "ordId": "452197707845865472",
            "ordType": "limit",
            "pnl": "0",
            "posSide": "",
            "px": "31527.1",
            "pxUsd":"",
            "pxVol":"",
            "pxType":"",
            "quickMgnType": "",
            "rebate": "0",
            "rebateCcy": "BTC",
            "reduceOnly": "false",
            "reqId": "",
            "side": "sell",
            "attachAlgoClOrdId": "",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "last",
            "source": "",
            "state": "filled",
            "stpId": "",
            "stpMode": "",
            "sz": "0.001",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "last",
            "attachAlgoOrds": [],
            "tradeId": "242589207",
            "tradeQuoteCcy": "USDT",
            "lastPx": "38892.2",
            "uTime": "1654084353264",
            "isTpLimit": "false",
            "linkedAlgoOrd": {
                "algoId": ""
            }
        }
    ]
}
```

### Push data parameters

| Parameter     | Type    | Description        |
|---------------|---------|--------------------|
| arg           | Object  | Subscribed channel |
| data          | Array   | Subscribed data    |

arg Structure:

| Parameter     | Type   | Description        |
|---------------|--------|--------------------|
| channel       | String | Channel name       |
| uid           | String | User Identifier    |
| instType      | String | Instrument type    |
| instFamily    | String | Instrument family  |
| instId        | String | Instrument ID      |

data Structure (many fields):

| Field         | Type    | Description                                 |
|---------------|---------|---------------------------------------------|
| instType      | String  | Instrument type                             |
| instId        | String  | Instrument ID                               |
| tgtCcy        | String  | Order quantity unit setting (base_ccy/quote_ccy)|
| ccy           | String  | Margin currency                             |
| ordId         | String  | Order ID                                    |
| clOrdId       | String  | Client Order ID                             |
| tag           | String  | Order tag                                   |
| px            | String  | Price                                       |
| pxUsd         | String  | Options price in USD                        |
| pxVol         | String  | Implied volatility                          |
| pxType        | String  | Price type                                  |
| sz            | String  | Order quantity                              |
| notionalUsd   | String  | Notional value in USD                       |
| ordType       | String  | Order type                                  |
| side          | String  | buy/sell                                    |
| posSide       | String  | Position side: net/long/short               |
| tdMode        | String  | Trade mode (cross/isolated/cash)            |
| fillPx        | String  | Last filled price                           |
| tradeId       | String  | Last trade ID                               |
| fillSz        | String  | Last filled quantity                        |
| fillPnl       | String  | Last filled profit and loss                 |
| fillTime      | String  | Last filled time                            |
| fillFee       | String  | Fee or rebate amount                        |
| fillFeeCcy    | String  | Fee or rebate currency                      |
| fillPxVol     | String  | Implied volatility when filled (options)    |
| fillPxUsd     | String  | Options price when filled (options)         |
| fillMarkVol   | String  | Mark volatility when filled (options)       |
| fillFwdPx     | String  | Forward price when filled (options)         |
| fillMarkPx    | String  | Mark price when filled                      |
| fillIdxPx     | String  | Index price at moment of trade execution    |
| execType      | String  | Taker (T) or Maker (M)                      |
| accFillSz     | String  | Accumulated fill quantity                   |
| fillNotionalUsd|String  | Filled notional value in USD                |
| avgPx         | String  | Average filled price                        |
| state         | String  | Order state                                 |
| lever         | String  | Leverage                                    |
| attachAlgoClOrdId|String| Client-supplied Algo ID (TP/SL)             |
| tpTriggerPx   | String  | TP trigger price                            |
| tpTriggerPxType| String | TP trigger price type                       |
| tpOrdPx       | String  | TP order price                              |
| slTriggerPx   | String  | SL trigger price                            |
| slTriggerPxType| String | SL trigger price type                       |
| slOrdPx       | String  | SL order price                              |
| attachAlgoOrds| Array   | TP/SL info attached on order                |
| linkedAlgoOrd | Object  | Linked SL order details                     |
| stpId         | String  | Self trade prevention ID                    |
| stpMode       | String  | Self trade prevention mode                  |
| feeCcy        | String  | Fee currency                                |
| fee           | String  | Fee and rebate                              |
| rebateCcy     | String  | Rebate currency                             |
| rebate        | String  | Rebate accumulated amount                   |
| pnl           | String  | Profit and loss                             |
| source        | String  | Order source                                |
| cancelSource  | String  | Order cancellation source                   |
| amendSource   | String  | Order amendation source                     |
| category      | String  | Category                                    |
| isTpLimit     | String  | TP limit order (true/false)                 |
| uTime         | String  | Update time (ms)                            |
| cTime         | String  | Creation time (ms)                          |
| reqId         | String  | Client request ID for amendment             |
| amendResult   | String  | Amendment result                            |
| reduceOnly    | String  | Reduce only (true/false)                    |
| quickMgnType  | String  | Quick margin type                           |
| algoClOrdId   | String  | Client-supplied Algo ID                     |
| algoId        | String  | Algo ID                                     |
| lastPx        | String  | Last price                                  |
| code          | String  | Error Code                                  |
| msg           | String  | Error Message                               |
| tradeQuoteCcy | String  | The quote currency used for trading         |



# WS / Place order

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is shared with the Place order REST API endpoints

### Request Example

```
{
  "id": "1512",
  "op": "order",
  "args": [
    {
      "side": "buy",
      "instId": "BTC-USDT",
      "tdMode": "cash",
      "ordType": "market",
      "sz": "100"
    }
  ]
}
```

### Request Parameters

| Parameter   | Type   | Required | Description                                  |
|-------------|--------|----------|----------------------------------------------|
| id          | String | Yes      | Unique identifier of the message             |
| op          | String | Yes      | Operation: order                            |
| args        | Array  | Yes      | Request parameters                           |

args Structure:

| Parameter  | Type      | Required | Description                                  |
|------------|-----------|----------|----------------------------------------------|
| instId     | String    | Yes      | Instrument ID, e.g. BTC-USDT                 |
| tdMode     | String    | Yes      | Trade mode: cash                             |
| clOrdId    | String    | No       | Client Order ID as assigned by the client     |
| tag        | String    | No       | Order tag                                    |
| side       | String    | Yes      | Order side: buy or sell                      |
| ordType    | String    | Yes      | Order type: market, limit, post_only, fok, ioc|
| sz         | String    | Yes      | Quantity to buy or sell                      |
| px         | String    | Conditional | Order price (see note)                   |
| tgtCcy     | String    | No       | Order quantity unit (base_ccy/quote_ccy)     |
| banAmend   | Boolean   | No       | Disallow system from amending SPOT market order|
| stpId      | String    | No       | Self trade prevention ID (deprecated)        |
| stpMode    | String    | No       | Self trade prevention mode.                  |
| expTime    | String    | No       | Effective deadline (timestamp, ms)            |
| tradeQuoteCcy|String   | No       | Quote currency used for trading (SPOT only)  |

### Response Example (success)

```
{
  "id": "1512",
  "op": "order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Failure Response Example

```
{
  "id": "1512",
  "op": "order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "",
      "tag": "",
      "sCode": "5XXXX",
      "sMsg": "not exist"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Response Example When Format Error

```
{
  "id": "1512",
  "op": "order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Response Parameters

| Parameter   | Type   | Description                                    |
|-------------|--------|------------------------------------------------|
| id          | String | Unique identifier of the message               |
| op          | String | Operation                                      |
| code        | String | Error Code                                     |
| msg         | String | Error Message                                  |
| data        | Array  | Data                                           |
| inTime      | String | Gateway received timestamp (microseconds)      |
| outTime     | String | Gateway response sent timestamp (microseconds) |

data Structure:

| Field    | Type   | Description                                    |
|----------|--------|------------------------------------------------|
| ordId    | String | Order ID                                       |
| clOrdId  | String | Client Order ID as assigned by the client      |
| tag      | String | Order tag                                      |
| sCode    | String | Status code (0 means success)                  |
| sMsg     | String | Event execution success/failure message        |


# WS / Place multiple orders

Place orders in a batch. Maximum 20 orders/request.  
URL Path  
`/ws/v5/private` (required login)

Rate Limit: 300 orders per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is determined by number of orders. If one order, consumes rate limit of Place order. Shared with Place multiple orders REST API.

### Request Example

```
{
  "id": "1513",
  "op": "batch-orders",
  "args": [
    {
      "side": "buy",
      "instId": "BTC-USDT",
      "tdMode": "cash",
      "ordType": "market",
      "sz": "100"
    },
    {
      "side": "buy",
      "instId": "LTC-USDT",
      "tdMode": "cash",
      "ordType": "market",
      "sz": "1"
    }
  ]
}
```

### Request Parameters

| Parameter  | Type   | Required | Description                              |
|------------|--------|----------|------------------------------------------|
| id         | String | Yes      | Unique identifier of the message         |
| op         | String | Yes      | Operation: batch-orders                  |
| args       | Array  | Yes      | Request Parameters                       |

args Structure:

| Field     | Type     | Required | Description                              |
|-----------|----------|----------|------------------------------------------|
| instId    | String   | Yes      | Instrument ID                            |
| tdMode    | String   | Yes      | Trade mode                               |
| clOrdId   | String   | No       | Client Order ID                          |
| tag       | String   | No       | Order tag                                |
| side      | String   | Yes      | Order side: buy or sell                  |
| ordType   | String   | Yes      | Order type (market, limit, post_only, fok, ioc)|
| sz        | String   | Yes      | Quantity to buy or sell                  |
| px        | String   | Conditional | Order price (see note)                |
| tgtCcy    | String   | No       | Order quantity unit (base_ccy/quote_ccy) |
| banAmend  | Boolean  | No       | Disallow system amending SPOT market order|
| stpId     | String   | No       | Self trade prevention ID (deprecated)    |
| stpMode   | String   | No       | Self trade prevention mode               |
| expTime   | String   | No       | Effective deadline (timestamp, ms)        |
| tradeQuoteCcy|String | No       | Quote currency for trading (SPOT only)   |

### Response Examples

**When All Succeed**

```
{
  "id": "1513",
  "op": "batch-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "",
      "ordId": "12344",
      "tag": "",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When Partially Successful**

```
{
  "id": "1513",
  "op": "batch-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "",
      "ordId": "",
      "tag": "",
      "sCode": "5XXXX",
      "sMsg": "Insufficient margin"
    }
  ],
  "code": "2",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When All Failed**

```
{
  "id": "1513",
  "op": "batch-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "",
      "tag": "",
      "sCode": "5XXXX",
      "sMsg": "Insufficient margin"
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "",
      "tag": "",
      "sCode": "5XXXX",
      "sMsg": "Insufficient margin"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When Format Error**

```
{
  "id": "1513",
  "op": "batch-orders",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Response Parameters

| Parameter   | Type   | Description                                    |
|-------------|--------|------------------------------------------------|
| id          | String | Unique identifier of the message               |
| op          | String | Operation                                      |
| code        | String | Error Code                                     |
| msg         | String | Error Message                                  |
| data        | Array  | Data                                           |
| inTime      | String | Gateway received timestamp (microseconds)      |
| outTime     | String | Gateway response sent timestamp (microseconds) |

data Structure:

| Field    | Type   | Description                                    |
|----------|--------|------------------------------------------------|
| ordId    | String | Order ID                                       |
| clOrdId  | String | Client Order ID as assigned by the client      |
| tag      | String | Order tag                                      |
| sCode    | String | Status code (0 means success)                  |
| sMsg     | String | Event execution success/failure message        |





Here is your API documentation content from the attached file, structured with proper Markdown tables for the tabular data parts. The text content remains unchanged:

***

# WS / Cancel order

Cancel an incomplete order

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is shared with the `Cancel order` REST API endpoints  

### Request Example

```
{
  "id": "1514",
  "op": "cancel-order",
  "args": [
    {
      "instId": "BTC-USDT",
      "ordId": "2510789768709120"
    }
  ]
}
```

### Request Parameters

| Parameter | Type   | Required    | Description                                                      |
|-----------|--------|-------------|------------------------------------------------------------------|
| id        | String | Yes         | Unique identifier of the message                                 |
| op        | String | Yes         | Operation: cancel-order                                          |
| args      | Array  | Yes         | Request Parameters                                               |

args structure:

| Parameter | Type   | Required    | Description                                                      |
|-----------|--------|-------------|------------------------------------------------------------------|
| instId    | String | Yes         | Instrument ID                                                   |
| ordId     | String | Conditional | Order ID (Either ordId or clOrdId is required; if both, ordId is used) |
| clOrdId   | String | Conditional | Client Order ID (as assigned by client)                         |

### Successful Response Example

```
{
  "id": "1514",
  "op": "cancel-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Failure Response Example

```
{
  "id": "1514",
  "op": "cancel-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "sCode": "5XXXX",
      "sMsg": "Order not exist"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Response Example When Format Error

```
{
  "id": "1514",
  "op": "cancel-order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Response Parameters

| Parameter | Type   | Description                                   |
|-----------|--------|-----------------------------------------------|
| id        | String | Unique identifier of the message               |
| op        | String | Operation                                      |
| code      | String | Error Code                                     |
| msg       | String | Error message                                  |
| data      | Array  | Data                                           |

data Structure:

| Parameter | Type   | Description                                   |
|-----------|--------|-----------------------------------------------|
| ordId     | String | Order ID                                      |
| clOrdId   | String | Client Order ID                               |
| sCode     | String | Order status code (0 means success)           |
| sMsg      | String | Order status message                           |

| inTime    | String | Timestamp at WebSocket gateway when request received (microseconds)   |
| outTime   | String | Timestamp at WebSocket gateway when response sent (microseconds)      |

> Cancel order returns with sCode equal to 0, meaning your cancellation request has been accepted by the system server. The result is subject to the state pushed by the order channel or the get order state.

***

# WS / Cancel multiple orders

Cancel incomplete orders in batches (max 20 orders per request).

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 300 orders per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is determined by number of orders. Shared with the `Cancel multiple orders` REST API endpoints.

### Request Example

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "args": [
    {
      "instId": "BTC-USDT",
      "ordId": "2517748157541376"
    },
    {
      "instId": "LTC-USDT",
      "ordId": "2517748155771904"
    }
  ]
}
```

### Request Parameters

| Parameter | Type   | Required | Description                                              |
|-----------|--------|----------|----------------------------------------------------------|
| id        | String | Yes      | Unique identifier of the message                         |
| op        | String | Yes      | Operation: batch-cancel-orders                           |
| args      | Array  | Yes      | Request Parameters                                       |

args structure:

| Parameter | Type   | Required    | Description                                             |
|-----------|--------|-------------|---------------------------------------------------------|
| instId    | String | Yes         | Instrument ID                                          |
| ordId     | String | Conditional | Order ID (Either ordId or clOrdId is required; if both, ordId is used) |
| clOrdId   | String | Conditional | Client Order ID as assigned by client                  |

### Response Examples

**When All Succeed**

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "2517748157541376",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "2517748155771904",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When Partially Successful**

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "2517748157541376",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "2517748155771904",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "2",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When All Failed**

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "2517748157541376",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "2517748155771904",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When Format Error**

```
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Response Parameters

| Parameter | Type   | Description                                   |
|-----------|--------|-----------------------------------------------|
| id        | String | Unique identifier of the message              |
| op        | String | Operation                                     |
| code      | String | Error Code                                    |
| msg       | String | Error message                                 |
| data      | Array  | Data                                          |

data Structure:

| Parameter | Type   | Description                                   |
|-----------|--------|-----------------------------------------------|
| ordId     | String | Order ID                                     |
| clOrdId   | String | Client Order ID                              |
| sCode     | String | Order status code (0 means success)           |
| sMsg      | String | Order status message                          |

| inTime    | String | Timestamp at WebSocket gateway when request received (microseconds)  |
| outTime   | String | Timestamp at WebSocket gateway when response sent (microseconds)      |

***

# WS / Amend order

Amend an incomplete order.

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 60 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is shared with the `Amend order` REST API endpoints

### Request Example

```
{
  "id": "1512",
  "op": "amend-order",
  "args": [
    {
      "instId": "BTC-USDT",
      "ordId": "2510789768709120",
      "newSz": "2"
    }
  ]
}
```

### Request Parameters

| Parameter | Type   | Required    | Description                                            |
|-----------|--------|-------------|--------------------------------------------------------|
| id        | String | Yes         | Unique identifier of the message                       |
| op        | String | Yes         | Operation: amend-order                                 |
| args      | Array  | Yes         | Request Parameters                                     |

args structure:

| Parameter | Type   | Required    | Description                                              |
|-----------|--------|-------------|----------------------------------------------------------|
| instId    | String | Yes         | Instrument ID                                           |
| cxlOnFail | Boolean| No          | Cancel order automatically on amend failure            |
| ordId     | String | Conditional | Order ID (either ordId or clOrdId required; ordId prioritized)|
| clOrdId   | String | Conditional | Client Order ID                                         |
| reqId     | String | No          | Client Request ID for amendment                         |
| newSz     | String | Conditional | New quantity after amendment (either newSz or newPx required)|
| newPx     | String | Conditional | New price after amendment                               |
| expTime   | String | No          | Request effective deadline (Unix timestamp in ms)       |

### Successful Response Example

```
{
  "id": "1512",
  "op": "amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Failure Response Example

```
{
  "id": "1512",
  "op": "amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "reqId": "b12344",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Response Example When Format Error

```
{
  "id": "1512",
  "op": "amend-order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Response Parameters

| Parameter | Type   | Description                                   |
|-----------|--------|-----------------------------------------------|
| id        | String | Unique identifier of the message              |
| op        | String | Operation                                     |
| code      | String | Error Code                                    |
| msg       | String | Error message                                 |
| data      | Array  | Data                                          |

data Structure:

| Parameter | Type   | Description                                   |
|-----------|--------|-----------------------------------------------|
| ordId     | String | Order ID                                     |
| clOrdId   | String | Client Order ID                              |
| reqId     | String | Client Request ID                            |
| sCode     | String | Order status code (0 means success)           |
| sMsg      | String | Order status message                          |

| inTime    | String | Timestamp at WebSocket gateway when request received (microseconds)  |
| outTime   | String | Timestamp at WebSocket gateway when response sent (microseconds)      |

> Note: If the new quantity of a partially-filled order is less than or equal to the filled quantity, the order status changes to "filled".



# WS / Amend multiple orders

Amend incomplete orders in batch (max 20 orders per request).

URL Path  
`/ws/v5/private` (required login)

Rate Limit: 300 orders per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Rate limit is shared with the `Amend multiple orders` REST API endpoints.

### Request Example

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "args": [
    {
      "instId": "BTC-USDT",
      "ordId": "12345689",
      "newSz": "2"
    },
    {
      "instId": "BTC-USDT",
      "ordId": "12344",
      "newSz": "2"
    }
  ]
}
```

### Request Parameters

| Parameter | Type   | Required | Description                                          |
|-----------|--------|----------|------------------------------------------------------|
| id        | String | Yes      | Unique identifier of the message                     |
| op        | String | Yes      | Operation: batch-amend-orders                         |
| args      | Array  | Yes      | Request parameters                                   |

args structure:

| Parameter | Type   | Required    | Description                                           |
|-----------|--------|-------------|-------------------------------------------------------|
| instId    | String | Yes         | Instrument ID                                       |
| cxlOnFail | Boolean| No          | Cancel order automatically on amend failure         |
| ordId     | String | Conditional | Order ID (either ordId or clOrdId required; ordId prioritized) |
| clOrdId   | String | Conditional | Client Order ID                                     |
| reqId     | String | No          | Client Request ID for amendment                     |
| newSz     | String | Conditional | New quantity after amendment (either newSz or newPx required)|
| newPx     | String | Conditional | New price after amendment                           |
| expTime   | String | No          | Request effective deadline (timestamp ms)           |

### Response Examples

**When All Succeed**

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "12345689",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "12344",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When All Failed**

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "reqId": "b12344",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "",
      "reqId": "b12344",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When Partially Successful**

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "",
      "reqId": "b12344",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "2",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

**When Format Error**

```
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

### Response Parameters

| Parameter | Type   | Description                                           |
|-----------|--------|-------------------------------------------------------|
| id        | String | Unique identifier of the message                      |
| op        | String | Operation                                             |
| code      | String | Error Code                                            |
| msg       | String | Error message                                         |
| data      | Array  | Data                                                  |

data Structure:

| Parameter | Type   | Description                                           |
|-----------|--------|-------------------------------------------------------|
| ordId     | String | Order ID                                            |
| clOrdId   | String | Client Order ID                                     |
| reqId     | String | Client Request ID for amendment                     |
| sCode     | String | Order status code (0 means success)                   |
| sMsg      | String | Order status message                                |

| inTime    | String | Timestamp at WebSocket gateway when request received (microseconds) |
| outTime   | String | Timestamp at WebSocket gateway when response sent (microseconds)     |

***

# Algo Trading

## POST / Place algo order

The algo order includes trigger order, OCO order, conditional order, and trailing order.

Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Permission: Trade  

### HTTP Request  

`POST /api/v5/trade/order-algo`

### Request Examples

**Place Take Profit / Stop Loss Order**

```
POST /api/v5/trade/order-algo
{
    "instId":"BTC-USDT",
    "tdMode":"cash",
    "side":"buy",
    "ordType":"conditional",
    "sz":"2",
    "tpTriggerPx":"15",
    "tpOrdPx":"18"
}
```

**Place Trigger Order**

```
POST /api/v5/trade/order-algo
{
    "instId": "BTC-USDT",
    "tdMode": "cash",
    "side": "buy",
    "ordType": "trigger",
    "sz": "10",
    "triggerPx": "100",
    "orderPx": "-1"
}
```

**Place Trailing Stop Order**

```
POST /api/v5/trade/order-algo
{
    "instId": "BTC-USDT",
    "tdMode": "cash",
    "side": "buy",
    "ordType": "move_order_stop",
    "sz": "10",
    "callbackRatio": "0.05"
}
```

### Request Parameters

| Parameter   | Type   | Required | Description                                    |
|-------------|--------|----------|------------------------------------------------|
| instId      | String | Yes      | Instrument ID, e.g. BTC-USDT                   |
| tdMode      | String | Yes      | Trade mode: cash                               |
| side        | String | Yes      | Order side: buy or sell                        |
| ordType     | String | Yes      | Order type: conditional, oco, trigger, move_order_stop |
| sz          | String | Yes      | Quantity to buy or sell                        |
| tag         | String | No       | Order tag (up to 16 characters)               |
| tgtCcy      | String | No       | Order quantity unit setting for sz            |
| algoClOrdId | String | No       | Client-supplied Algo ID                        |

### Take Profit / Stop Loss Order Parameters

| Parameter      | Type   | Required | Description                                      |
|----------------|--------|----------|--------------------------------------------------|
| tpTriggerPx    | String | No       | Take-profit trigger price (must accompany tpOrdPx) |
| tpTriggerPxType| String | No       | Take-profit trigger price type (default "last")  |
| tpOrdPx        | String | No       | Take-profit order price (must accompany tpTriggerPx) |
| slTriggerPx    | String | No       | Stop-loss trigger price (must accompany slOrdPx)|
| slTriggerPxType| String | No       | Stop-loss trigger price type (default "last")    |
| slOrdPx        | String | No       | Stop-loss order price (must accompany slTriggerPx)|

> When placing net TP/SL order (ordType=conditional) with both TP and SL parameters, only stop-loss logic will be performed; take-profit logic will be ignored.

***

# POST / Cancel algo order

Cancel unfilled algo orders. Max 10 orders per request.

Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Permission: Trade  

### HTTP Request

`POST /api/v5/trade/cancel-algos`

### Request Example

```
POST /api/v5/trade/cancel-algos
[
    {
        "algoId":"198273485",
        "instId":"BTC-USDT"
    },
    {
        "algoId":"198273485",
        "instId":"BTC-USDT"
    }
]
```

### Request Parameters

| Parameter | Type   | Required | Description               |
|-----------|--------|----------|---------------------------|
| algoId    | String | Yes      | Algo ID                   |
| instId    | String | Yes      | Instrument ID             |

### Response Example

```
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "1836489397437468672",
            "clOrdId": "",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

### Response Parameters

| Parameter    | Type   | Description                           |
|--------------|--------|-------------------------------------|
| algoId       | String | Algo ID                            |
| sCode        | String | Event execution code (0 means success) |
| sMsg         | String | Rejection message if unsuccessful   |
| clOrdId      | String | Client Order ID (Deprecated)         |
| algoClOrdId  | String | Client-supplied Algo ID (Deprecated) |
| tag          | String | Order tag (Deprecated)               |

***

# POST / Amend algo order

Amend unfilled algo orders (Stop order and Trigger order only).

Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID + Instrument ID  
Permission: Trade  

### HTTP Request

`POST /api/v5/trade/amend-algos`

### Request Example

```
POST /api/v5/trade/amend-algos
{
    "algoId":"2510789768709120",
    "newSz":"2",
    "instId":"BTC-USDT"
}
```

### Request Parameters

| Parameter       | Type    | Required    | Description                                         |
|-----------------|---------|-------------|-----------------------------------------------------|
| instId          | String  | Yes         | Instrument ID                                      |
| algoId          | String  | Conditional | Algo ID (Either algoId or algoClOrdId required)  |
| algoClOrdId     | String  | Conditional | Client-supplied Algo ID                            |
| cxlOnFail       | Boolean | No          | Cancel if amendment fails (default false)         |
| reqId           | String  | Conditional | Client Request ID for amendment                    |
| newSz           | String  | Conditional | New quantity after amendment (must be > 0)        |
| newTpTriggerPx  | String  | Conditional | New take-profit trigger price                      |
| newTpOrdPx      | String  | Conditional | New take-profit order price                         |
| newSlTriggerPx  | String  | Conditional | New stop-loss trigger price                         |
| newSlOrdPx      | String  | Conditional | New stop-loss order price                           |
| newTpTriggerPxType | String | Conditional | Take-profit trigger price type                     |
| newSlTriggerPxType | String | Conditional | Stop-loss trigger price type                        |

### Response Example

```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId":"algo_01",
            "algoId":"2510789768709120",
            "reqId":"po103ux",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

### Response Parameters

| Parameter   | Type   | Description                                     |
|-------------|--------|------------------------------------------------|
| algoId      | String | Algo ID                                        |
| algoClOrdId | String | Client-supplied Algo ID                        |
| reqId       | String | Client Request ID for amendment                |
| sCode       | String | Event execution result (0 means success)       |
| sMsg        | String | Rejection message if request unsuccessful      |

***

# GET / Algo order details

Rate Limit: 20 requests per 2 seconds  
Rate limit rule: User ID  
Permission: Read  

## HTTP Request

`GET /api/v5/trade/order-algo`

### Request Example

`GET /api/v5/trade/order-algo?algoId=1753184812254216192`

### Request Parameters

| Parameter   | Type   | Required    | Description                             |
|-------------|--------|-------------|-----------------------------------------|
| algoId      | String | Conditional | Algo ID (Either algoId or algoClOrdId is required)|
| algoClOrdId | String | Conditional | Client-supplied Algo ID                 |

### Response Example

```
{
    "code": "0",
    "data": [
        {
            "activePx": "",
            "actualPx": "",
            "actualSide": "",
            "actualSz": "0",
            "algoClOrdId": "",
            "algoId": "1753184812254216192",
            "amendPxOnTriggerType": "0",
            "attachAlgoOrds": [],
            "cTime": "1724751378980",
            ...
            "triggerPx": "",
            "triggerPxType": "",
            "triggerTime": "",
            ...
            "tpTriggerPx": "10000",
            "tpTriggerPxType": "last",
            "tpOrdPx": "-1",
            ...
            "state": "live",
            ...
        }
    ],
    "msg": ""
}
```

### Response Parameters (selected)

| Parameter        | Type    | Description                                         |
|------------------|---------|-----------------------------------------------------|
| instType         | String  | Instrument type                                    |
| instId           | String  | Instrument ID                                      |
| ccy              | String  | Margin currency (applicable to margin orders)      |
| ordId            | String  | Latest order ID (deprecated soon)                   |
| ordIdList        | Array   | Order ID list (multiple if TP/SL splitting order)  |
| algoId           | String  | Algo ID                                            |
| clOrdId          | String  | Client Order ID                                    |
| sz               | String  | Quantity to buy or sell                            |
| closeFraction    | String  | Fraction of position to close on algo trigger     |
| ordType          | String  | Order type                                        |
| side             | String  | Order side                                        |
| posSide          | String  | Position side                                     |
| tdMode           | String  | Trade mode                                        |
| tgtCcy           | String  | Order quantity unit setting for sz                |
| state            | String  | State (live, pause, effective, canceled, order_failed...) |
| lever            | String  | Leverage (0.01 to 125)                            |
| tpTriggerPx      | String  | Take-profit trigger price                          |
| tpTriggerPxType  | String  | Take-profit trigger price type                     |
| tpOrdPx          | String  | Take-profit order price                            |
| slTriggerPx      | String  | Stop-loss trigger price                            |
| slTriggerPxType  | String  | Stop-loss trigger price type                       |
| slOrdPx          | String  | Stop-loss order price                              |
| triggerPx        | String  | Trigger price                                     |
| triggerPxType    | String  | Trigger price type                                |
| ordPx            | String  | Order price for trigger order                      |
| actualSz         | String  | Actual order quantity                              |
| actualPx         | String  | Actual order price                                 |
| tag              | String  | Order tag                                         |
| actualSide       | String  | Actual trigger side (tp/sl) (only for oco and conditional)|
| triggerTime      | String  | Trigger time (ms)                                 |
| pxVar            | String  | Price ratio (for iceberg/twap orders)             |
| pxSpread         | String  | Price variance (for iceberg/twap orders)          |
| szLimit          | String  | Average amount (for iceberg/twap orders)          |
| pxLimit          | String  | Price limit (for iceberg/twap orders)             |
| timeInterval     | String  | Time interval (for twap orders)                    |
| callbackRatio    | String  | Callback price ratio (for move_order_stop order)  |
| callbackSpread   | String  | Callback price variance (for move_order_stop order)|
| activePx         | String  | Active price (for move_order_stop order)           |
| moveTriggerPx    | String  | Trigger price (for move_order_stop order)          |
| reduceOnly       | String  | Whether order can only reduce position size         |
| quickMgnType     | String  | Quick Margin type                                  |
| last             | String  | Last filled price while placing order               |
| failCode         | String  | Algo order fail reason (empty if not failed)        |
| algoClOrdId      | String  | Client-supplied Algo ID                             |
| amendPxOnTriggerType | String | Enable cost-price SL for split TPs               |
| attachAlgoOrds   | Array   | Attached SL/TP orders info                         |
| cTime            | String  | Creation time (ms)                                 |
| uTime            | String  | Update time (ms)                                   |
| isTradeBorrowMode | String | Whether borrowing currency automatically<br>true<br>false<br>Only applicable to trigger order, trailing order and twap order |
| chaseType | String | Chase type. Only applicable to chase order. |
| chaseVal | String | Chase value. Only applicable to chase order. |
| maxChaseType | String | Maximum chase type. Only applicable to chase order. |
| maxChaseVal | String | Maximum chase value. Only applicable to chase order. |
| tradeQuoteCcy | String | The quote currency used for trading. |

## GET / Algo order list

Retrieve a list of untriggered Algo orders under the current account.

**Rate Limit:** 20 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/trade/orders-algo-pending
```

### Request Example
```
GET /api/v5/trade/orders-algo-pending?ordType=conditional
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ordType | String | Yes | Order type<br>conditional: One-way stop order<br>oco: One-cancels-the-other order<br>trigger: Trigger order<br>move_order_stop: Trailing order |
| algoId | String | No | Algo ID |
| instType | String | No | Instrument type<br>SPOT |
| instId | String | No | Instrument ID, e.g. BTC-USDT |
| after | String | No | Pagination of data to return records earlier than the requested algoId. |
| before | String | No | Pagination of data to return records newer than the requested algoId. |
| limit | String | No | Number of results per request. The maximum is 100. The default is 100 |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "activePx": "",
            "actualPx": "",
            "actualSide": "buy",
            "actualSz": "0",
            "algoClOrdId": "",
            "algoId": "681096944655273984",
            "amendPxOnTriggerType": "",
            "attachAlgoOrds": [],
            "cTime": "1708658165774",
            "callbackRatio": "",
            "callbackSpread": "",
            "ccy": "",
            "clOrdId": "",
            "closeFraction": "",
            "failCode": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "last": "51014.6",
            "lever": "",
            "moveTriggerPx": "",
            "ordId": "",
            "ordIdList": [],
            "ordPx": "-1",
            "ordType": "trigger",
            "posSide": "net",
            "pxLimit": "",
            "pxSpread": "",
            "pxVar": "",
            "quickMgnType": "",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "state": "live",
            "sz": "10",
            "szLimit": "",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "timeInterval": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "triggerPx": "100",
            "triggerPxType": "last",
            "triggerTime": "0"
        }
    ],
    "msg": ""
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instType | String | Instrument type |
| instId | String | Instrument ID |
| ccy | String | Margin currency<br>Applicable to all isolated MARGIN orders and cross MARGIN orders in Futures mode. |
| ordId | String | Latest order ID |
| ordIdList | Array of strings | Order ID list. There will be multiple order IDs when there is TP/SL splitting order. |
| algoId | String | Algo ID |
| clOrdId | String | Client Order ID as assigned by the client |
| sz | String | Quantity to buy or sell |
| closeFraction | String | Fraction of position to be closed when the algo order is triggered |
| ordType | String | Order type |
| side | String | Order side |
| posSide | String | Position side |
| tdMode | String | Trade mode |
| tgtCcy | String | Order quantity unit setting for sz<br>base_ccy: Base currency ,quote_ccy: Quote currency<br>Only applicable to SPOT traded with Market order |
| state | String | State,live pause |
| lever | String | Leverage, from 0.01 to 125.<br>Only applicable to MARGIN/FUTURES/SWAP |
| tpTriggerPx | String | Take-profit trigger price |
| tpTriggerPxType | String | Take-profit trigger price type.<br>last: last price<br>index: index price<br>mark: mark price |
| tpOrdPx | String | Take-profit order price |
| slTriggerPx | String | Stop-loss trigger price |
| slTriggerPxType | String | Stop-loss trigger price type.<br>last: last price<br>index: index price<br>mark: mark price |
| slOrdPx | String | Stop-loss order price |
| triggerPx | String | Trigger price |
| triggerPxType | String | Trigger price type<br>last: last price<br>index: index price<br>mark: mark price |
| ordPx | String | Order price for the trigger order |
| actualSz | String | Actual order quantity |
| tag | String | Order tag |
| actualPx | String | Actual order price |
| actualSide | String | Actual trigger side<br>tp: take profit sl: stop loss<br>Only applicable to oco order and conditional order |
| triggerTime | String | Trigger time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| pxVar | String | Price ratio<br>Only applicable to iceberg order or twap order |
| pxSpread | String | Price variance<br>Only applicable to iceberg order or twap order |
| szLimit | String | Average amount<br>Only applicable to iceberg order or twap order |
| pxLimit | String | Price Limit<br>Only applicable to iceberg order or twap order |
| timeInterval | String | Time interval<br>Only applicable to twap order |
| callbackRatio | String | Callback price ratio<br>Only applicable to move_order_stop order |
| callbackSpread | String | Callback price variance<br>Only applicable to move_order_stop order |
| activePx | String | Active price<br>Only applicable to move_order_stop order |
| moveTriggerPx | String | Trigger price<br>Only applicable to move_order_stop order |
| reduceOnly | String | Whether the order can only reduce the position size. Valid options: true or false. |
| quickMgnType | String | Quick Margin type, Only applicable to Quick Margin Mode of isolated margin<br>manual, auto_borrow, auto_repay |
| last | String | Last filled price while placing |
| failCode | String | It represents that the reason that algo order fails to trigger. There will be value when the state is order_failed, e.g. 51008;<br>For this endpoint, it always is "". |
| algoClOrdId | String | Client-supplied Algo ID |
| amendPxOnTriggerType | String | Whether to enable Cost-price SL. Only applicable to SL order of split TPs.<br>0: disable, the default value<br>1: Enable |
| attachAlgoOrds | Array of objects | Attached SL/TP orders info<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |

#### Nested attachAlgoOrds Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| > attachAlgoClOrdId | String | Client-supplied Algo ID when placing order attaching TP/SL.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > tpTriggerPx | String | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well. |
| > tpTriggerPxType | String | Take-profit trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>The default is last |
| > tpOrdPx | String | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well.<br>If the price is -1, take-profit will be executed at the market price. |
| > slTriggerPx | String | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price. |
| > slTriggerPxType | String | Stop-loss trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>The default is last |
| > slOrdPx | String | Stop-loss order price<br>If you fill in this parameter, you should fill in the stop-loss trigger price.<br>If the price is -1, stop-loss will be executed at the market price. |
| cTime | String | Creation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |

## GET / Algo order history

Retrieve a list of all algo orders under the current account in the last 3 months.

**Rate Limit:** 20 requests per 2 seconds  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/trade/orders-algo-history
```

### Request Example
```
GET /api/v5/trade/orders-algo-history?ordType=conditional&state=effective
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ordType | String | Yes | Order type<br>conditional: One-way stop order<br>oco: One-cancels-the-other order<br>trigger: Trigger order<br>move_order_stop: Trailing order |
| state | String | Conditional | State<br>effective<br>canceled<br>order_failed<br>Either state or algoId is required |
| algoId | String | Conditional | Algo ID<br>Either state or algoId is required. |
| instType | String | No | Instrument type<br>SPOT |
| instId | String | No | Instrument ID, e.g. BTC-USDT |
| after | String | No | Pagination of data to return records earlier than the requested algoId |
| before | String | No | Pagination of data to return records new than the requested algoId |
| limit | String | No | Number of results per request. The maximum is 100. The default is 100 |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "activePx": "",
            "actualPx": "",
            "actualSide": "buy",
            "actualSz": "0",
            "algoClOrdId": "",
            "algoId": "681096944655273984",
            "amendPxOnTriggerType": "",
            "attachAlgoOrds": [],
            "cTime": "1708658165774",
            "callbackRatio": "",
            "callbackSpread": "",
            "ccy": "",
            "clOrdId": "",
            "closeFraction": "",
            "failCode": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "last": "51014.6",
            "lever": "",
            "moveTriggerPx": "",
            "ordId": "",
            "ordIdList": [],
            "ordPx": "-1",
            "ordType": "trigger",
            "posSide": "net",
            "pxLimit": "",
            "pxSpread": "",
            "pxVar": "",
            "quickMgnType": "",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "state": "canceled",
            "sz": "10",
            "szLimit": "",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "timeInterval": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "triggerPx": "100",
            "triggerPxType": "last",
            "triggerTime": ""
        }
    ],
    "msg": ""
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instType | String | Instrument type |
| instId | String | Instrument ID |
| ccy | String | Margin currency<br>Applicable to all isolated MARGIN orders and cross MARGIN orders in Futures mode. |
| ordId | String | Latest order ID |
| ordIdList | Array of strings | Order ID list. There will be multiple order IDs when there is TP/SL splitting order. |
| algoId | String | Algo ID |
| clOrdId | String | Client Order ID as assigned by the client |
| sz | String | Quantity to buy or sell |
| closeFraction | String | Fraction of position to be closed when the algo order is triggered |
| ordType | String | Order type |
| side | String | Order side |
| posSide | String | Position side |
| tdMode | String | Trade mode |
| tgtCcy | String | Order quantity unit setting for sz<br>base_ccy: Base currency ,quote_ccy: Quote currency<br>Only applicable to SPOT Market Orders<br>Default is quote_ccy for buy, base_ccy for sell |
| state | String | State<br>effective<br>canceled<br>order_failed<br>partially_failed |
| lever | String | Leverage, from 0.01 to 125.<br>Only applicable to MARGIN/FUTURES/SWAP |
| tpTriggerPx | String | Take-profit trigger price. |
| tpTriggerPxType | String | Take-profit trigger price type.<br>last: last price<br>index: index price<br>mark: mark price |
| tpOrdPx | String | Take-profit order price. |
| slTriggerPx | String | Stop-loss trigger price. |
| slTriggerPxType | String | Stop-loss trigger price type.<br>last: last price<br>index: index price<br>mark: mark price |
| slOrdPx | String | Stop-loss order price. |
| triggerPx | String | trigger price. |
| triggerPxType | String | trigger price type.<br>last: last price<br>index: index price<br>mark: mark price |
| ordPx | String | Order price for the trigger order |
| actualSz | String | Actual order quantity |
| actualPx | String | Actual order price |
| tag | String | Order tag |
| actualSide | String | Actual trigger side, tp: take profit sl: stop loss<br>Only applicable to oco order and conditional order |
| triggerTime | String | Trigger time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| pxVar | String | Price ratio<br>Only applicable to iceberg order or twap order |
| pxSpread | String | Price variance<br>Only applicable to iceberg order or twap order |
| szLimit | String | Average amount<br>Only applicable to iceberg order or twap order |
| pxLimit | String | Price Limit<br>Only applicable to iceberg order or twap order |
| timeInterval | String | Time interval<br>Only applicable to twap order |
| callbackRatio | String | Callback price ratio<br>Only applicable to move_order_stop order |
| callbackSpread | String | Callback price variance<br>Only applicable to move_order_stop order |
| activePx | String | Active price<br>Only applicable to move_order_stop order |
| moveTriggerPx | String | Trigger price<br>Only applicable to move_order_stop order |
| reduceOnly | String | Whether the order can only reduce the position size. Valid options: true or false. |
| quickMgnType | String | Quick Margin type, Only applicable to Quick Margin Mode of isolated margin<br>manual, auto_borrow, auto_repay |
| last | String | Last filled price while placing |
| failCode | String | It represents that the reason that algo order fails to trigger. It is "" when the state is effective/canceled. There will be value when the state is order_failed, e.g. 51008;<br>Only applicable to Stop Order, Trailing Stop Order, Trigger order. |
| algoClOrdId | String | Client Algo Order ID as assigned by the client. |
| amendPxOnTriggerType | String | Whether to enable Cost-price SL. Only applicable to SL order of split TPs.<br>0: disable, the default value<br>1: Enable |
| attachAlgoOrds | Array of objects | Attached SL/TP orders info<br>Applicable to Futures mode/Multi-currency margin/Portfolio margin |
| > attachAlgoClOrdId | String | Client-supplied Algo ID when placing order attaching TP/SL.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > tpTriggerPx | String | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well. |
| > tpTriggerPxType | String | Take-profit trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>The default is last |
| > tpOrdPx | String | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well.<br>If the price is -1, take-profit will be executed at the market price. |
| > slTriggerPx | String | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price. |
| > slTriggerPxType | String | Stop-loss trigger price type<br>last: last price<br>index: index price<br>mark: mark price<br>The default is last |
| > slOrdPx | String | Stop-loss order price<br>If you fill in this parameter, you should fill in the stop-loss trigger price.<br>If the price is -1, stop-loss will be executed at the market price. |
| cTime | String | Creation time Unix timestamp format in milliseconds, e.g. 1597026383085 |




# WS / Algo orders channel

Retrieve algo orders (trigger, OCO, conditional). Data pushed on updates only.

URL Path  
`/ws/v5/business` (required login)

### Request Example

```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "orders-algo",
      "instType": "ANY"
    }
  ]
}
```

### Request Parameters

| Parameter | Type   | Required | Description                           |
|-----------|--------|----------|-------------------------------------|
| id        | String | No       | Unique identifier                   |
| op        | String | Yes      | Operation (subscribe/unsubscribe)   |
| args      | Array  | Yes      | List of subscribed channels          |

args structure:

| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| channel   | String | Yes      | Channel name: orders-algo |
| instType  | String | Yes      | Instrument type   |
| instId    | String | No       | Instrument ID     |

### Successful Response Example

```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "orders-algo",
    "instType": "ANY"
  },
  "connId": "a4d3ae55"
}
```

### Failure Response Example

```
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"orders-algo\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

### Response Parameters

| Parameter | Type   | Required | Description                  |
|-----------|--------|----------|------------------------------|
| id        | String | No       | Unique identifier             |
| event     | String | Yes      | Event: subscribe/unsubscribe/error |
| arg       | Object | No       | Subscribed channel content    |
| code      | String | No       | Error code                   |
| msg       | String | No       | Error message                |
| connId    | String | Yes      | WebSocket connection ID      |

arg structure:

| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| channel   | String | Yes      | Channel name      |
| uid       | String | No       | User Identifier   |
| instType  | String | No       | Instrument type   |
| instFamily| String | No       | Instrument family |
| instId    | String | No       | Instrument ID     |

### Push Data Example

```
{
    "arg": {
        "channel": "orders-algo",
        "uid": "77982378738415879",
        "instType": "ANY"
    },
    "data": [{
        "actualPx": "0",
        "actualSide": "",
        "actualSz": "0",
        "algoClOrdId": "",
        "algoId": "581878926302093312",
        "attachAlgoOrds": [],
        "amendResult": "",
        "cTime": "1685002746818",
        "ccy": "",
        "clOrdId": "",
        "closeFraction": "",
        "failCode": "",
        "instId": "BTC-USDC",
        "instType": "SPOT",
        "last": "26174.8",
        "lever": "0",
        "notionalUsd": "11.0",
        "ordId": "",
        "ordIdList": [],
        "ordPx": "",
        "ordType": "conditional",
        "posSide": "",
        "quickMgnType": "",
        "reduceOnly": "false",
        "reqId": "",
        "side": "buy",
        "slOrdPx": "",
        "slTriggerPx": "",
        "slTriggerPxType": "",
        "state": "live",
        "sz": "11",
        "tag": "",
        "tdMode": "cross",
        "tgtCcy": "quote_ccy",
        "tpOrdPx": "-1",
        "tpTriggerPx": "1",
        "tpTriggerPxType": "last",
        "triggerPx": "",
        "triggerTime": "",
        "amendPxOnTriggerType": "0"
    }]
}
```

### Response Parameters when data is pushed

| Parameter   | Type    | Description                                                |
|-------------|---------|------------------------------------------------------------|
| arg         | Object  | Successfully subscribed channel                            |
| data        | Array   | Subscribed data                                            |


# OKX API Documentation - Public Data & Funding Account

## Public Data
The API endpoints of Public Data do not require authentication.

### REST API

## Get instruments
Retrieve a list of instruments with open contracts.

**Rate Limit:** 20 requests per 2 seconds  
**Rate limit rule:** IP + Instrument Type

### HTTP Request
```
GET /api/v5/public/instruments
```

### Request Example
```
GET /api/v5/public/instruments?instType=SPOT
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| instType | String | Yes | Instrument type<br>SPOT |
| instId | String | No | Instrument ID |

### Response Example
```
{
    "code":"0",
    "msg":"",
    "data":[
      {
            "alias": "",
            "auctionEndTime": "",
            "baseCcy": "BTC",
            "category": "1",
            "ctMult": "",
            "ctType": "",
            "ctVal": "",
            "ctValCcy": "",
            "expTime": "",
            "futureSettlement": false,
            "instFamily": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "lever": "10",
            "listTime": "1606468572000",
            "lotSz": "0.00000001",
            "maxIcebergSz": "9999999999.0000000000000000",
            "maxLmtAmt": "1000000",
            "maxLmtSz": "9999999999",
            "maxMktAmt": "1000000",
            "maxMktSz": "",
            "maxStopSz": "",
            "maxTriggerSz": "9999999999.0000000000000000",
            "maxTwapSz": "9999999999.0000000000000000",
            "minSz": "0.00001",
            "optType": "",
            "openType": "call_auction",
            "quoteCcy": "USDT",
            "settleCcy": "",
            "state": "live",
            "stk": "",
            "tickSz": "0.1",
            "uly": ""
        }
    ]
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| instType | String | Instrument type |
| instId | String | Instrument ID, e.g. BTC-USDT |
| uly | String | Underlying, e.g. BTC-USD<br>Only applicable to FUTURES/SWAP/OPTION |
| instFamily | String | Instrument family, e.g. BTC-USD<br>Only applicable to FUTURES/SWAP/OPTION |
| category | String | Currency category. Note: this parameter is already deprecated |
| baseCcy | String | Base currency, e.g. BTC inBTC-USDT<br>Only applicable to SPOT/MARGIN |
| quoteCcy | String | Quote currency, e.g. USDT in BTC-USDT<br>Only applicable to SPOT/MARGIN |
| settleCcy | String | Settlement and margin currency, e.g. BTC<br>Only applicable to FUTURES/SWAP/OPTION |
| ctVal | String | Contract value<br>Only applicable to FUTURES/SWAP/OPTION |
| ctMult | String | Contract multiplier<br>Only applicable to FUTURES/SWAP/OPTION |
| ctValCcy | String | Contract value currency<br>Only applicable to FUTURES/SWAP/OPTION |
| optType | String | Option type, C: Call P: put<br>Only applicable to OPTION |
| stk | String | Strike price<br>Only applicable to OPTION |
| listTime | String | Listing time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| auctionEndTime | String | The end time of call auction, Unix timestamp format in milliseconds, e.g. 1597026383085<br>Only applicable to SPOT that are listed through call auctions, return "" in other cases (deprecated, use contTdSwTime) |
| contTdSwTime | String | Continuous trading switch time. The switch time from call auction, prequote to continuous trading, Unix timestamp format in milliseconds. e.g. 1597026383085.<br>Only applicable to SPOT/MARGIN that are listed through call auction or prequote, return "" in other cases. |
| openType | String | Open type<br>fix_price: fix price opening<br>pre_quote: pre-quote<br>call_auction: call auction<br>Only applicable to SPOT/MARGIN, return "" for all other business lines |
| expTime | String | Expiry time<br>Applicable to SPOT/MARGIN/FUTURES/SWAP/OPTION. For FUTURES/OPTION, it is natural delivery/exercise time. It is the instrument offline time when there is SPOT/MARGIN/FUTURES/SWAP/ manual offline. Update once change. |
| lever | String | Max Leverage,<br>Not applicable to SPOT, OPTION |
| tickSz | String | Tick size, e.g. 0.0001<br>For Option, it is minimum tickSz among tick band, please use "Get option tick bands" if you want get option tickBands. |
| lotSz | String | Lot size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| minSz | String | Minimum order size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| ctType | String | Contract type<br>linear: linear contract<br>inverse: inverse contract<br>Only applicable to FUTURES/SWAP |
| alias | String | Alias<br>this_week<br>next_week<br>this_month<br>next_month<br>quarter<br>next_quarter<br>third_quarter<br>Only applicable to FUTURES<br>Not recommended for use, users are encouraged to rely on the expTime field to determine the delivery time of the contract |
| state | String | Instrument status<br>live<br>suspend<br>preopen e.g. Futures and options contracts rollover from generation to trading start; certain symbols before they go live<br>test: Test pairs, can't be traded |
| maxLmtSz | String | The maximum order quantity of a single limit order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| maxMktSz | String | The maximum order quantity of a single market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in USDT. |
| maxLmtAmt | String | Max USD amount for a single limit order |
| maxMktAmt | String | Max USD amount for a single market order<br>Only applicable to SPOT/MARGIN |
| maxTwapSz | String | The maximum order quantity of a single TWAP order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| maxIcebergSz | String | The maximum order quantity of a single iceBerg order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| maxTriggerSz | String | The maximum order quantity of a single trigger order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| maxStopSz | String | The maximum order quantity of a single stop market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in USDT. |
| futureSettlement | Boolean | Whether daily settlement for expiry feature is enabled<br>Applicable to FUTURES cross |

#### Notes on listTime and contTdSwTime
For spot symbols listed through a call auction or pre-open, listTime represents the start time of the auction or pre-open, and contTdSwTime indicates the end of the auction or pre-open and the start of continuous trading. For other scenarios, listTime will mark the beginning of continuous trading, and contTdSwTime will return an empty value "".

#### Notes on state
The state will always change from `preopen` to `live` when the listTime is reached.
When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument will not be available.

## Get system time
Retrieve API server time.

**Rate Limit:** 10 requests per 2 seconds  
**Rate limit rule:** IP

### HTTP Request
```
GET /api/v5/public/time
```

### Request Example
```
GET /api/v5/public/time
```

### Response Example
```
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "ts":"1597026383085"
    }
  ]
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ts | String | System time, Unix timestamp format in milliseconds, e.g. 1597026383085 |

## WebSocket

### Instruments channel

**URL Path:** `/ws/v5/public`

### Request Example
```
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "instruments",
      "instType": "SPOT"
    }
  ]
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message<br>Provided by client. It will be returned in response message for identifying the corresponding request.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>subscribe<br>unsubscribe |
| args | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name<br>instruments |
| > instType | String | Yes | Instrument type<br>SPOT |

### Successful Response Example
```
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "instruments",
    "instType": "SPOT"
  },
  "connId": "a4d3ae55"
}
```

### Failure Response Example
```
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"instruments\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

### Response parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message |
| event | String | Yes | Event<br>subscribe<br>unsubscribe<br>error |
| arg | Object | No | Subscribed channel |
| > channel | String | Yes | Channel name |
| > instType | String | Yes | Instrument type<br>SPOT |
| code | String | No | Error code |
| msg | String | No | Error message |
| connId | String | Yes | WebSocket connection ID |

### Push Data Example
```
{
  "arg": {
    "channel": "instruments",
    "instType": "SPOT"
  },
  "data": [
    {
        "alias": "",
        "baseCcy": "BTC",
        "category": "1",
        "ctMult": "",
        "ctType": "",
        "ctVal": "",
        "ctValCcy": "",
        "contTdSwTime": "1704876947000",
        "expTime": "",
        "instFamily": "",
        "instId": "BTC-USDT",
        "instType": "SPOT",
        "lever": "10",
        "listTime": "1606468572000",
        "lotSz": "0.00000001",
        "maxIcebergSz": "9999999999.0000000000000000",
        "maxLmtAmt": "1000000",
        "maxLmtSz": "9999999999",
        "maxMktAmt": "1000000",
        "maxMktSz": "",
        "maxStopSz": "",
        "maxTriggerSz": "9999999999.0000000000000000",
        "maxTwapSz": "9999999999.0000000000000000",
        "minSz": "0.00001",
        "optType": "",
        "openType": "call_auction",
        "quoteCcy": "USDT",
        "settleCcy": "",
        "state": "live",
        "stk": "",
        "tickSz": "0.1",
        "uly": ""
    }
  ]
}
```

### Push data parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| arg | Object | Subscribed channel |
| > channel | String | Channel name |
| > instType | String | Instrument type |
| data | Array of objects | Subscribed data |
| > instType | String | Instrument type |
| > instId | String | Instrument ID, e.g. BTC-UST |
| > uly | String | Underlying, e.g. BTC-USD<br>Only applicable to FUTURES/SWAP/OPTION |
| > instFamily | String | Instrument family, e.g. BTC-USD<br>Only applicable to FUTURES/SWAP/OPTION |
| > category | String | Currency category. Note: this parameter is already deprecated |
| > baseCcy | String | Base currency, e.g. BTC in BTC-USDT<br>Only applicable to SPOT/MARGIN |
| > quoteCcy | String | Quote currency, e.g. USDT in BTC-USDT<br>Only applicable to SPOT/MARGIN |
| > settleCcy | String | Settlement and margin currency, e.g. BTC<br>Only applicable to FUTURES/SWAP/OPTION |
| > ctVal | String | Contract value |
| > ctMult | String | Contract multiplier |
| > ctValCcy | String | Contract value currency |
| > optType | String | Option type<br>C: Call<br>P: Put<br>Only applicable to OPTION |
| > stk | String | Strike price<br>Only applicable to OPTION |
| > listTime | String | Listing time |
| > auctionEndTime | String | The end time of call auction, Unix timestamp format in milliseconds, e.g. 1597026383085<br>Only applicable to SPOT that are listed through call auctions, return "" in other cases (deprecated, use contTdSwTime) |
| > contTdSwTime | String | Continuous trading switch time. The switch time from call auction, prequote to continuous trading, Unix timestamp format in milliseconds. e.g. 1597026383085.<br>Only applicable to SPOT/MARGIN that are listed through call auction or prequote, return "" in other cases. |
| > openType | String | Open type<br>fix_price: fix price opening<br>pre_quote: pre-quote<br>call_auction: call auction<br>Only applicable to SPOT/MARGIN, return "" for all other business lines |
| > expTime | String | Expiry time<br>Applicable to SPOT/MARGIN/FUTURES/SWAP/OPTION. For FUTURES/OPTION, it is the delivery/exercise time. It can also be the delisting time of the trading instrument. Update once change. |
| > lever | String | Max Leverage<br>Not applicable to SPOT/OPTION, used to distinguish between MARGIN and SPOT. |
| > tickSz | String | Tick size, e.g. 0.0001<br>For Option, it is minimum tickSz among tick band. |
| > lotSz | String | Lot size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency |
| > minSz | String | Minimum order size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency |
| > ctType | String | Contract type<br>linear: linear contract<br>inverse: inverse contract<br>Only applicable to FUTURES/SWAP |
| > alias | String | Alias<br>this_week<br>next_week<br>this_month<br>next_month<br>quarter<br>next_quarter<br>Only applicable to FUTURES<br>Not recommended for use, users are encouraged to rely on the expTime field to determine the delivery time of the contract |
| > state | String | Instrument status<br>live<br>suspend<br>expired<br>preopen. e.g. There will be preopen before the Futures and Options new contracts state is live.<br>test: Test pairs, can't be traded |
| > maxLmtSz | String | The maximum order quantity of a single limit order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| > maxMktSz | String | The maximum order quantity of a single market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in USDT. |
| > maxTwapSz | String | The maximum order quantity of a single TWAP order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| > maxIcebergSz | String | The maximum order quantity of a single iceBerg order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| > maxTriggerSz | String | The maximum order quantity of a single trigger order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in base currency. |
| > maxStopSz | String | The maximum order quantity of a single stop market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is SPOT/MARGIN, the value is the quantity in USDT. |

#### Notes
Instrument status will trigger pushing of incremental data from instruments channel. When a new contract is going to be listed, the instrument data of the new contract will be available with status preopen. When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument status will be changed to expired.

#### Notes on listTime and contTdSwTime
For spot symbols listed through a call auction or pre-open, listTime represents the start time of the auction or pre-open, and contTdSwTime indicates the end of the auction or pre-open and the start of continuous trading. For other scenarios, listTime will mark the beginning of continuous trading, and contTdSwTime will return an empty value "".

---

# Funding Account
The API endpoints of Funding Account require authentication.

### REST API

## Get currencies
Retrieve a list of all currencies available which are related to the current account's KYC entity.

**Rate Limit:** 6 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/asset/currencies
```

### Request Example
```
GET /api/v5/asset/currencies
```

### Request Parameters

| Parameters | Types | Required | Description |
|------------|-------|----------|-------------|
| ccy | String | No | Single currency or multiple currencies separated with comma, e.g. BTC or BTC,ETH. |

### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "burningFeeRate": "",
        "canDep": true,
        "canInternal": true,
        "canWd": true,
        "ccy": "BTC",
        "chain": "BTC-Bitcoin",
        "ctAddr": "",
        "depEstOpenTime": "",
        "depQuotaFixed": "",
        "depQuoteDailyLayer2": "",
        "fee": "0.00005",
        "logoLink": "https://static.coinall.ltd/cdn/oksupport/asset/currency/icon/btc20230419112752.png",
        "mainNet": true,
        "maxFee": "0.00005",
        "maxFeeForCtAddr": "",
        "maxWd": "500",
        "minDep": "0.0005",
        "minDepArrivalConfirm": "1",
        "minFee": "0.00005",
        "minFeeForCtAddr": "",
        "minInternal": "0.0001",
        "minWd": "0.0005",
        "minWdUnlockConfirm": "2",
        "name": "Bitcoin",
        "needTag": false,
        "usedDepQuotaFixed": "",
        "usedWdQuota": "0",
        "wdEstOpenTime": "",
        "wdQuota": "10000000",
        "wdTickSz": "8"
    }
  ]
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency, e.g. BTC |
| name | String | Name of currency. There is no related name when it is not shown. |
| logoLink | String | The logo link of currency |
| chain | String | Chain name, e.g. USDT-ERC20, USDT-TRC20 |
| ctAddr | String | Contract address |
| canDep | Boolean | The availability to deposit from chain<br>false: not available<br>true: available |
| canWd | Boolean | The availability to withdraw to chain<br>false: not available<br>true: available |
| canInternal | Boolean | The availability to internal transfer<br>false: not available<br>true: available |
| depEstOpenTime | String | Estimated opening time for deposit, Unix timestamp format in milliseconds, e.g. 1597026383085<br>if canDep is true, it returns "" |
| wdEstOpenTime | String | Estimated opening time for withdraw, Unix timestamp format in milliseconds, e.g. 1597026383085<br>if canWd is true, it returns "" |
| minDep | String | The minimum deposit amount of currency in a single transaction |
| minWd | String | The minimum on-chain withdrawal amount of currency in a single transaction |
| minInternal | String | The minimum internal transfer amount of currency in a single transaction<br>No maximum internal transfer limit in a single transaction, subject to the withdrawal limit in the past 24 hours(wdQuota). |
| maxWd | String | The maximum amount of currency on-chain withdrawal in a single transaction |
| wdTickSz | String | The withdrawal precision, indicating the number of digits after the decimal point.<br>The withdrawal fee precision kept the same as withdrawal precision.<br>The accuracy of internal transfer withdrawal is 8 decimal places. |
| wdQuota | String | The withdrawal limit in the past 24 hours (including on-chain withdrawal and internal transfer), unit in USD |
| usedWdQuota | String | The amount of currency withdrawal used in the past 24 hours, unit in USD |
| fee | String | The fixed withdrawal fee<br>Apply to on-chain withdrawal |
| minFee | String | The minimum withdrawal fee for normal address<br>Apply to on-chain withdrawal<br>(Deprecated) |
| maxFee | String | The maximum withdrawal fee for normal address<br>Apply to on-chain withdrawal<br>(Deprecated) |
| minFeeForCtAddr | String | The minimum withdrawal fee for contract address<br>Apply to on-chain withdrawal<br>(Deprecated) |
| maxFeeForCtAddr | String | The maximum withdrawal fee for contract address<br>Apply to on-chain withdrawal<br>(Deprecated) |
| burningFeeRate | String | Burning fee rate, e.g "0.05" represents "5%".<br>Some currencies may charge combustion fees. The burning fee is deducted based on the withdrawal quantity (excluding gas fee) multiplied by the burning fee rate.<br>Apply to on-chain withdrawal |
| mainNet | Boolean | If current chain is main net, then it will return true, otherwise it will return false |
| needTag | Boolean | Whether tag/memo information is required for withdrawal, e.g. EOS will return true |
| minDepArrivalConfirm | String | The minimum number of blockchain confirmations to acknowledge fund deposit. The account is credited after that, but the deposit can not be withdrawn |
| minWdUnlockConfirm | String | The minimum number of blockchain confirmations required for withdrawal of a deposit |
| depQuotaFixed | String | The fixed deposit limit, unit in USD<br>Return empty string if there is no deposit limit |
| usedDepQuotaFixed | String | The used amount of fixed deposit quota, unit in USD<br>Return empty string if there is no deposit limit |
| depQuoteDailyLayer2 | String | The layer2 network daily deposit limit |

## Get balance
Retrieve the funding account balances of all the assets and the amount that is available or on hold.

**Note:** Only asset information of a currency with a balance greater than 0 will be returned.

**Rate Limit:** 6 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/asset/balances
```

### Request Example
```
GET /api/v5/asset/balances
```

### Request Parameters

| Parameters | Types | Required | Description |
|------------|-------|----------|-------------|
| ccy | String | No | Single currency or multiple currencies (no more than 20) separated with comma, e.g. BTC or BTC,ETH. |

### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "availBal": "37.11827078",
            "bal": "37.11827078",
            "ccy": "ETH",
            "frozenBal": "0"
        }
    ]
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency |
| bal | String | Balance |
| frozenBal | String | Frozen balance |
| availBal | String | Available balance |

## Get non-tradable assets
Retrieve the funding account balances of all the assets and the amount that is available or on hold.

**Rate Limit:** 6 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/asset/non-tradable-assets
```

### Request Example
```
GET /api/v5/asset/non-tradable-assets
```

### Request Parameters

| Parameters | Types | Required | Description |
|------------|-------|----------|-------------|
| ccy | String | No | Single currency or multiple currencies (no more than 20) separated with comma, e.g. BTC or BTC,ETH. |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "bal": "989.84719571",
            "burningFeeRate": "",
            "canWd": true,
            "ccy": "CELT",
            "chain": "CELT-OKTC",
            "ctAddr": "f403fb",
            "fee": "2",
            "feeCcy": "USDT",
            "logoLink": "https://static.coinall.ltd/cdn/assets/imgs/221/460DA8A592400393.png",
            "minWd": "0.1",
            "name": "",
            "needTag": false,
            "wdAll": false,
            "wdTickSz": "8"
        },
        {
            "bal": "0.001",
            "burningFeeRate": "",
            "canWd": true,
            "ccy": "MEME",
            "chain": "MEME-ERC20",
            "ctAddr": "09b760",
            "fee": "5",
            "feeCcy": "USDT",
            "logoLink": "https://static.coinall.ltd/cdn/assets/imgs/207/2E664E470103C613.png",
            "minWd": "0.001",
            "name": "MEME Inu",
            "needTag": false,
            "wdAll": false,
            "wdTickSz": "8"
        }
    ],
    "msg": ""
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency, e.g. CELT |
| name | String | Chinese name of currency. There is no related name when it is not shown. |
| logoLink | String | Logo link of currency |
| bal | String | Withdrawable balance |
| canWd | Boolean | Availability to withdraw to chain.<br>false: not available true: available |
| chain | String | Chain for withdrawal |
| minWd | String | Minimum withdrawal amount of currency in a single transaction |
| wdAll | Boolean | Whether all assets in this currency must be withdrawn at one time |
| fee | String | Fixed withdrawal fee |
| feeCcy | String | Fixed withdrawal fee unit, e.g. USDT |
| burningFeeRate | String | Burning fee rate, e.g "0.05" represents "5%".<br>Some currencies may charge combustion fees. The burning fee is deducted based on the withdrawal quantity (excluding gas fee) multiplied by the burning fee rate. |
| ctAddr | String | Last 6 digits of contract address |
| wdTickSz | String | Withdrawal precision, indicating the number of digits after the decimal point |
| needTag | Boolean | Whether tag/memo information is required for withdrawal |

## Get account asset valuation
View account asset valuation

**Rate Limit:** 1 request per second  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/asset/asset-valuation
```

### Request Example
```
GET /api/v5/asset/asset-valuation
```

### Request Parameters

| Parameters | Types | Required | Description |
|------------|-------|----------|-------------|
| ccy | String | No | Asset valuation calculation unit<br>BTC, USDT<br>USD, CNY, JP, KRW, RUB, EUR<br>VND, IDR, INR, PHP, THB, TRY<br>AUD, SGD, ARS, SAR, AED, IQD<br>The default is the valuation in BTC. |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "details": {
                "classic": "124.6",
                "earn": "1122.73",
                "funding": "0.09",
                "trading": "2544.28"
            },
            "totalBal": "3790.09",
            "ts": "1637566660769"
        }
    ],
    "msg": ""
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| totalBal | String | Valuation of total account assets |
| ts | String | Unix timestamp format in milliseconds, e.g.1597026383085 |
| details | Object | Asset valuation details for each account |
| > funding | String | Funding account |
| > trading | String | Trading account |
| > classic | String | [Deprecated] Classic account |
| > earn | String | Earn account |

## Funds transfer
Only API keys with Trade privilege can call this endpoint.

This endpoint supports the transfer of funds between your funding account and trading account, and from the master account to sub-accounts.

Sub-account can transfer out to master account by default. Need to call Set permission of transfer out to grant privilege first if you want sub-account transferring to another sub-account (sub-accounts need to belong to same master account.)

**Note:** The success or failure of the request does not necessarily reflect the actual transfer result. Recommend checking the transfer status by calling "Get funds transfer state" to confirm the final result.

**Rate Limit:** 2 requests per second  
**Rate limit rule:** User ID + Currency  
**Permission:** Trade

### HTTP Request
```
POST /api/v5/asset/transfer
```

### Request Example

```bash
# Transfer 1.5 USDT from funding account to Trading account when current account is master-account
POST /api/v5/asset/transfer
body
{
    "ccy":"USDT",
    "amt":"1.5",
    "from":"6",
    "to":"18"
}

# Transfer 1.5 USDT from funding account to subAccount when current account is master-account
POST /api/v5/asset/transfer
body
{
    "ccy":"USDT",
    "type":"1",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "subAcct":"mini"
}

# Transfer 1.5 USDT from funding account to subAccount when current account is sub-account
POST /api/v5/asset/transfer
body 
{
    "ccy":"USDT",
    "type":"4",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "subAcct":"mini"
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| type | String | No | Transfer type<br>0: transfer within account<br>1: master account to sub-account (Only applicable to API Key from master account)<br>2: sub-account to master account (Only applicable to API Key from master account)<br>3: sub-account to master account (Only applicable to APIKey from sub-account)<br>4: sub-account to sub-account (Only applicable to APIKey from sub-account, and target account needs to be another sub-account which belongs to same master account. Sub-account directly transfer out permission is disabled by default, set permission please refer to Set permission of transfer out)<br>The default is 0.<br>If you want to make transfer between sub-accounts by master account API key, refer to Master accounts manage the transfers between sub-accounts |
| ccy | String | Yes | Transfer currency, e.g. USDT |
| amt | String | Yes | Amount to be transferred |
| from | String | Yes | The remitting account<br>6: Funding account<br>18: Trading account |
| to | String | Yes | The beneficiary account<br>6: Funding account<br>18: Trading account |
| subAcct | String | Conditional | Name of the sub-account<br>When type is 1/2/4, this parameter is required. |
| loanTrans | Boolean | No | Whether or not borrowed coins can be transferred out under Spot mode/Multi-currency margin/Portfolio margin<br>true: borrowed coins can be transferred out<br>false: borrowed coins cannot be transferred out<br>the default is false |
| omitPosRisk | String | No | Ignore position risk<br>Default is false<br>Applicable to Portfolio margin |
| clientId | String | No | Client-supplied ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |

### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "transId": "754147",
      "ccy": "USDT",
      "clientId": "",
      "from": "6",
      "amt": "0.1",
      "to": "18"
    }
  ]
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| transId | String | Transfer ID |
| clientId | String | Client-supplied ID |
| ccy | String | Currency |
| from | String | The remitting account |
| amt | String | Transfer amount |
| to | String | The beneficiary account |

## Get funds transfer state
Retrieve the transfer state data of the last 2 weeks.

**Rate Limit:** 10 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/asset/transfer-state
```

### Request Example
```
GET /api/v5/asset/transfer-state?transId=1&type=1
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| transId | String | Conditional | Transfer ID<br>Either transId or clientId is required. If both are passed, transId will be used. |
| clientId | String | Conditional | Client-supplied ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| type | String | No | Transfer type<br>0: transfer within account<br>1: master account to sub-account (Only applicable to API Key from master account)<br>2: sub-account to master account (Only applicable to API Key from master account)<br>3: sub-account to master account (Only applicable to APIKey from sub-account)<br>4: sub-account to sub-account (Only applicable to APIKey from sub-account, and target account needs to be another sub-account which belongs to same master account)<br>The default is 0.<br>For Custody accounts, can choose not to pass this parameter or pass 0. |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "amt": "1.5",
            "ccy": "USDT",
            "clientId": "",
            "from": "18",
            "instId": "", //deprecated
            "state": "success",
            "subAcct": "test",
            "to": "6",
            "toInstId": "", //deprecated
            "transId": "1",
            "type": "1"
        }
    ],
    "msg": ""
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| transId | String | Transfer ID |
| clientId | String | Client-supplied ID |
| ccy | String | Currency, e.g. USDT |
| amt | String | Amount to be transferred |
| type | String | Transfer type<br>0: transfer within account<br>1: master account to sub-account (Only applicable to API Key from master account)<br>2: sub-account to master account (Only applicable to APIKey from master account)<br>3: sub-account to master account (Only applicable to APIKey from sub-account)<br>4: sub-account to sub-account (Only applicable to APIKey from sub-account, and target account needs to be another sub-account which belongs to same master account) |
| from | String | The remitting account<br>6: Funding account<br>18: Trading account |
| to | String | The beneficiary account<br>6: Funding account<br>18: Trading account |
| subAcct | String | Name of the sub-account |
| instId | String | deprecated |
| toInstId | String | deprecated |
| state | String | Transfer state<br>success<br>pending<br>failed |

## Asset bills details
Query the billing record in the past month.

**Rate Limit:** 6 Requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/asset/bills
```

### Request Example
```
GET /api/v5/asset/bills
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | No | Currency |
| type | String | No | Bill type<br>1: Deposit<br>2: Withdrawal<br>13: Canceled withdrawal<br>20: Transfer to sub account (for master account)<br>21: Transfer from sub account (for master account)<br>22: Transfer out from sub to master account (for sub-account)<br>23: Transfer in from master to sub account (for sub-account)<br>28: Manually claimed Airdrop<br>47: System reversal<br>48: Event Reward<br>49: Event Giveaway<br>68: Fee rebate (by rebate card)<br>72: Token received<br>73: Token given away<br>74: Token refunded<br>75: [Simple earn flexible] Subscription<br>76: [Simple earn flexible] Redemption<br>77: Jumpstart distribute<br>78: Jumpstart lock up<br>80: DEFI/Staking subscription<br>82: DEFI/Staking redemption<br>83: Staking yield<br>84: Violation fee<br>89: Deposit yield<br>116: [Fiat] Place an order<br>117: [Fiat] Fulfill an order<br>118: [Fiat] Cancel an order<br>124: Jumpstart unlocking<br>130: Transferred from Trading account<br>131: Transferred to Trading account<br>132: [P2P] Frozen by customer service<br>133: [P2P] Unfrozen by customer service<br>134: [P2P] Transferred by customer service<br>135: Cross chain exchange<br>137: [ETH Staking] Subscription<br>138: [ETH Staking] Swapping<br>139: [ETH Staking] Earnings<br>146: Customer feedback<br>150: Affiliate commission<br>151: Referral reward<br>152: Broker reward<br>160: Dual Investment subscribe<br>161: Dual Investment collection<br>162: Dual Investment profit<br>163: Dual Investment refund<br>172: [Affiliate] Sub-affiliate commission<br>173: [Affiliate] Fee rebate (by trading fee)<br>174: Jumpstart Pay<br>175: Locked collateral<br>176: Loan<br>177: Added collateral<br>178: Returned collateral<br>179: Repayment<br>180: Unlocked collateral<br>181: Airdrop payment<br>185: [Broker] Convert reward<br>187: [Broker] Convert transfer<br>189: Mystery box bonus<br>195: Untradable asset withdrawal<br>196: Untradable asset withdrawal revoked<br>197: Untradable asset deposit<br>198: Untradable asset collection reduce<br>199: Untradable asset collection increase<br>200: Buy<br>202: Price Lock Subscribe<br>203: Price Lock Collection<br>204: Price Lock Profit<br>205: Price Lock Refund<br>207: Dual Investment Lite Subscribe<br>208: Dual Investment Lite Collection<br>209: Dual Investment Lite Profit<br>210: Dual Investment Lite Refund<br>212: [Flexible loan] Multi-collateral loan collateral locked<br>215: [Flexible loan] Multi-collateral loan collateral released<br>217: [Flexible loan] Multi-collateral loan borrowed<br>218: [Flexible loan] Multi-collateral loan repaid<br>232: [Flexible loan] Subsidized interest received<br>220: Delisted crypto<br>221: Blockchain's withdrawal fee<br>222: Withdrawal fee refund<br>223: SWAP lead trading profit share<br>225: Shark Fin subscribe<br>226: Shark Fin collection<br>227: Shark Fin profit<br>228: Shark Fin refund<br>229: Airdrop<br>232: Subsidized interest received<br>233: Broker rebate compensation<br>240: Snowball subscribe<br>241: Snowball refund<br>242: Snowball profit<br>243: Snowball trading failed<br>249: Seagull subscribe<br>250: Seagull collection<br>251: Seagull profit<br>252: Seagull refund<br>263: Strategy bots profit share<br>265: Signal revenue<br>266: SPOT lead trading profit share<br>270: DCD broker transfer<br>271: DCD broker rebate<br>272: [Convert] Buy Crypto/Fiat<br>273: [Convert] Sell Crypto/Fiat<br>284: [Custody] Transfer out trading sub-account<br>285: [Custody] Transfer in trading sub-account<br>286: [Custody] Transfer out custody funding account<br>287: [Custody] Transfer in custody funding account<br>288: [Custody] Fund delegation<br>289: [Custody] Fund undelegation<br>299: Affiliate recommendation commission<br>300: Fee discount rebate<br>303: Snowball market maker transfer<br>304: [Simple Earn Fixed] Order submission<br>305: [Simple Earn Fixed] Order redemption<br>306: [Simple Earn Fixed] Principal distribution<br>307: [Simple Earn Fixed] Interest distribution (early termination compensation)<br>308: [Simple Earn Fixed] Interest distribution<br>309: [Simple Earn Fixed] Interest distribution (extension compensation)<br>311: Crypto dust auto-transfer in<br>313: Sent by gift<br>314: Received from gift<br>315: Refunded from gift<br>328: [SOL staking] Send Liquidity Staking Token reward<br>329: [SOL staking] Subscribe Liquidity Staking Token staking<br>330: [SOL staking] Mint Liquidity Staking Token<br>331: [SOL staking] Redeem Liquidity Staking Token order<br>332: [SOL staking] Settle Liquidity Staking Token order<br>333: Trial fund reward<br>339: [Simple Earn Fixed] Order submission<br>340: [Simple Earn Fixed] Order failure refund<br>341: [Simple Earn Fixed] Redemption<br>342: [Simple Earn Fixed] Principal<br>343: [Simple Earn Fixed] Interest<br>344: [Simple Earn Fixed] Compensatory interest<br>345: [Institutional Loan] Principal repayment<br>346: [Institutional Loan] Interest repayment<br>347: [Institutional Loan] Overdue penalty<br>348: [BTC staking] Subscription<br>349: [BTC staking] Redemption<br>350: [BTC staking] Earnings<br>351: [Institutional Loan] Loan disbursement<br>354: Copy and bot rewards<br>361: Deposit from closed sub-account<br>372: Asset segregation<br>373: Asset release |
| clientId | String | No | Client-supplied ID for transfer or withdrawal<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| after | String | No | Pagination of data to return records earlier than the requested ts or billId, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| before | String | No | Pagination of data to return records newer than the requested ts or billId, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| limit | String | No | Number of results per request. The maximum is 100. The default is 100. |
| pagingType | String | No | PagingType<br>1: Timestamp of the bill record<br>2: Bill ID of the bill record<br>The default is 1 |

### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "billId": "12344",
        "ccy": "BTC",
        "clientId": "",
        "balChg": "2",
        "bal": "12",
        "type": "1",
        "ts": "1597026383085",
        "notes": ""
    }]
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| billId | String | Bill ID |
| ccy | String | Account balance currency |
| clientId | String | Client-supplied ID for transfer or withdrawal |
| balChg | String | Change in balance at the account level |
| bal | String | Balance at the account level |
| type | String | Bill type |
| notes | String | Notes |
| ts | String | Creation time, Unix timestamp format in milliseconds, e.g.1597026383085 |

## Asset bills history
Query the billing records of all time since 1 February, 2021.

**⚠️ IMPORTANT:** Data updates occur every 30 seconds. Update frequency may vary based on data volume - please be aware of potential delays during high-traffic periods.

**Rate Limit:** 1 Requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/asset/bills-history
```

### Request Example
```
GET /api/v5/asset/bills-history
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | No | Currency |
| type | String | No | Bill type<br>[Same extensive list as Asset bills details above] |
| clientId | String | No | Client-supplied ID for transfer or withdrawal<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| after | String | No | Pagination of data to return records earlier than the requested ts or billId, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| before | String | No | Pagination of data to return records newer than the requested ts or billId, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| limit | String | No | Number of results per request. The maximum is 100. The default is 100. |
| pagingType | String | No | PagingType<br>1: Timestamp of the bill record<br>2: Bill ID of the bill record<br>The default is 1 |

### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "billId": "12344",
        "ccy": "BTC",
        "clientId": "",
        "balChg": "2",
        "bal": "12",
        "type": "1",
        "ts": "1597026383085",
        "notes": ""
    }]
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| billId | String | Bill ID |
| ccy | String | Account balance currency |
| clientId | String | Client-supplied ID for transfer or withdrawal |
| balChg | String | Change in balance at the account level |
| bal | String | Balance at the account level |
| type | String | Bill type |
| notes | String | Notes |
| ts | String | Creation time, Unix timestamp format in milliseconds, e.g.1597026383085 |

## Get deposit address
Retrieve the deposit addresses of currencies, including previously-used addresses.

**Rate Limit:** 6 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/asset/deposit-address
```

### Request Example
```
GET /api/v5/asset/deposit-address?ccy=BTC
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | Yes | Currency, e.g. BTC |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "chain": "BTC-Bitcoin",
            "ctAddr": "",
            "ccy": "BTC",
            "to": "6",
            "addr": "39XNxK1Ryqgg3Bsyn6HzoqV4Xji25pNkv6",
            "verifiedName":"John Corner",
            "selected": true
        },
        {
            "chain": "BTC-OKC",
            "ctAddr": "",
            "ccy": "BTC",
            "to": "6",
            "addr": "0x66d0edc2e63b6b992381ee668fbcb01f20ae0428",
            "verifiedName":"John Corner",
            "selected": true
        },
        {
            "chain": "BTC-ERC20",
            "ctAddr": "5807cf",
            "ccy": "BTC",
            "to": "6",
            "addr": "0x66d0edc2e63b6b992381ee668fbcb01f20ae0428",
            "verifiedName":"John Corner",
            "selected": true
        }
    ],
    "msg": ""
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| addr | String | Deposit address |
| tag | String | Deposit tag (This will not be returned if the currency does not require a tag for deposit) |
| memo | String | Deposit memo (This will not be returned if the currency does not require a memo for deposit) |
| pmtId | String | Deposit payment ID (This will not be returned if the currency does not require a payment_id for deposit) |
| addrEx | Object | Deposit address attachment (This will not be returned if the currency does not require this)<br>e.g. TONCOIN attached tag name is comment, the return will be {'comment':'123456'} |
| ccy | String | Currency, e.g. BTC |
| chain | String | Chain name, e.g. USDT-ERC20, USDT-TRC20 |
| to | String | The beneficiary account<br>6: Funding account 18: Trading account<br>The users under some entity (e.g. Brazil) only support deposit to trading account. |
| verifiedName | String | Verified name (for recipient) |
| selected | Boolean | Return true if the current deposit address is selected by the website page |
| ctAddr | String | Last 6 digits of contract address |

## Get deposit history
Retrieve the deposit records according to the currency, deposit status, and time range in reverse chronological order. The 100 most recent records are returned by default.
Websocket API is also available, refer to Deposit info channel.

**Rate Limit:** 6 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

### HTTP Request
```
GET /api/v5/asset/deposit-history
```

### Request Example
```bash
GET /api/v5/asset/deposit-history

# Query deposit history from 2022-06-01 to 2022-07-01
GET /api/v5/asset/deposit-history?ccy=BTC&after=1654041600000&before=1656633600000
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | No | Currency, e.g. BTC |
| depId | String | No | Deposit ID |
| fromWdId | String | No | Internal transfer initiator's withdrawal ID<br>If the deposit comes from internal transfer, this field displays the withdrawal ID of the internal transfer initiator |
| txId | String | No | Hash record of the deposit |
| type | String | No | Deposit Type<br>3: internal transfer<br>4: deposit from chain |
| state | String | No | Status of deposit<br>0: waiting for confirmation<br>1: deposit credited<br>2: deposit successful<br>8: pending due to temporary deposit suspension on this crypto currency<br>11: match the address blacklist<br>12: account or deposit is frozen<br>13: sub-account deposit interception<br>14: KYC limit<br>17: Pending response from Travel Rule vendor |
| after | String | No | Pagination of data to return records earlier than the requested ts, Unix timestamp format in milliseconds, e.g. 1654041600000 |
| before | String | No | Pagination of data to return records newer than the requested ts, Unix timestamp format in milliseconds, e.g. 1656633600000 |
| limit | string | No | Number of results per request. The maximum is 100; The default is 100 |

### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "actualDepBlkConfirm": "2",
        "amt": "1",
        "areaCodeFrom": "",
        "ccy": "USDT",
        "chain": "USDT-TRC20",
        "depId": "88****33",
        "from": "",
        "fromWdId": "",
        "state": "2",
        "to": "TN4hGjVXMzy*********9b4N1aGizqs",
        "ts": "1674038705000",
        "txId": "fee235b3e812********857d36bb0426917f0df1802"
    }
  ]
}
```

### Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency |
| chain | String | Chain name |
| amt | String | Deposit amount |
| from | String | Deposit account<br>If the deposit comes from an internal transfer, this field displays the account information of the internal transfer initiator, which can be a mobile phone number, email address, account name, and will return "" in other cases |
| areaCodeFrom | String | If from is a phone number, this parameter return area code of the phone number |
| to | String | Deposit address<br>If the deposit comes from the on-chain, this field displays the on-chain address, and will return "" in other cases |
| txId | String | Hash record of the deposit |
| ts | String | The timestamp that the deposit record is created, Unix timestamp format in milliseconds, e.g. 1655251200000 |
| state | String | Status of deposit<br>0: Waiting for confirmation<br>1: Deposit credited<br>2: Deposit successful<br>8: Pending due to temporary deposit suspension on this crypto currency<br>11: Match the address blacklist<br>12: Account or deposit is frozen<br>13: Sub-account deposit interception<br>14: KYC limit |
| depId | String | Deposit ID |
| fromWdId | String | Internal transfer initiator's withdrawal ID<br>If the deposit comes from internal transfer, this field displays the withdrawal ID of the internal transfer initiator, and will return "" in other cases |
| actualDepBlkConfirm | String | The actual amount of blockchain confirmed in a single deposit |

#### About deposit state
- **Waiting for confirmation** is that the required number of blockchain confirmations has not been reached.
- **Deposit credited** is that there is sufficient number of blockchain confirmations for the currency to be credited to the account, but it cannot be withdrawn yet.
- **Deposit successful** means the crypto has been credited to the account and it can be withdrawn.

## Withdrawal
Only supported withdrawal of assets from funding account. Common sub-account does not support withdrawal.

**Note:** The API can only make withdrawal to verified addresses/account, and verified addresses can be set by WEB/APP.

#### About tag
Some token deposits require a deposit address and a tag (e.g. Memo/Payment ID), which is a string that guarantees the uniqueness of your deposit address. Follow the deposit procedure carefully, or you may risk losing your assets.
For currencies with labels, if it is a withdrawal between OKX users, please use internal transfer instead of online withdrawal

#### API withdrawal restrictions for EEA users
Due to recent updates to the Travel Rule regulations within the EEA, API withdrawal to private wallet is not allowed and the user can continue to withdraw crypto through our website or app. If the user withdraw to the exchange wallet, relevant information about the recipient must be provided. More details please refer to https://eea.okx.com/docs-v5/log_en/#2025-01-14-withdrawal-api-adjustment-for-eea-entity-users

**Rate Limit:** 6 requests per second  
**Rate limit rule:** User ID  
**Permission:** Withdraw

### HTTP Request
```
POST /api/v5/asset/withdrawal
```

### Request Example
```bash
# Withdrawal to exchange wallet
POST /api/v5/asset/withdrawal
body
{
    "amt": "1000",
    "dest": "4",
    "ccy": "USDT",
    "toAddr": "TYW7C5qjhQtuhjz5qLQtA9bzaeEFo6ueg7",
    "chain": "USDT-TRC20",
    "rcvrInfo": {
        "walletType": "exchange",
        "exchId":"did:ethr:0x401c4bc0241bc787f64510f21a4ae7ec3603487c",
        "rcvrFirstName":"Bruce",
        "rcvrLastName":"Wayne",
        "rcvrCountry":"United States",
        "rcvrStreetName":"Clementi Avenue 1",
        "rcvrTownName":"San Jose",
        "rcvrCountrySubDivision":"California"
    }
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | Yes | Currency, e.g. USDT |
| amt | String | Yes | Withdrawal amount<br>Withdrawal fee is not included in withdrawal amount. Please reserve sufficient transaction fees when withdrawing.<br>You can get fee amount by Get currencies.<br>For internal transfer, transaction fee is always 0. |
| dest | String | Yes | Withdrawal method<br>3: internal transfer<br>4: on-chain withdrawal |
| toAddr | String | Yes | toAddr should be a trusted address/account.<br>If your dest is 4, some crypto currency addresses are formatted as 'address:tag', e.g. 'ARDOR-7JF3-8F2E-QUWZ-CAN7F:123456'<br>If your dest is 3,toAddr should be a recipient address which can be UID, email, phone or login account name (account name is only for sub-account). |
| toAddrType | String | No | Address type<br>1: wallet address, email, phone, or login account name<br>2: UID (only applicable for dest=3) |
| chain | String | Conditional | Chain name<br>There are multiple chains under some currencies, such as USDT has USDT-ERC20, USDT-TRC20<br>If the parameter is not filled in, the default will be the main chain.<br>When you withdrawal the non-tradable asset, if the parameter is not filled in, the default will be the unique withdrawal chain.<br>Apply to on-chain withdrawal.<br>You can get supported chain name by the endpoint of Get currencies. |
| areaCode | String | Conditional | Area code for the phone number, e.g. 86<br>If toAddr is a phone number, this parameter is required.<br>Apply to internal transfer |
| rcvrInfo | Object | Conditional | Recipient information<br>For the specific entity users to do on-chain withdrawal/lightning withdrawal, this information is required. |
| > walletType | String | Yes | Wallet Type<br>exchange: Withdraw to exchange wallet<br>private: Withdraw to private wallet<br>For the wallet belongs to business recipient, rcvrFirstName may input the company name, rcvrLastName may input "N/A", location info may input the registered address of the company. |
| > exchId | String | Conditional | Exchange ID<br>You can query supported exchanges through the endpoint of Get exchange list (public)<br>If the exchange is not in the exchange list, fill in '0' in this field.<br>Apply to walletType = exchange |
| > rcvrFirstName | String | Conditional | Receiver's first name, e.g. Bruce |
| > rcvrLastName | String | Conditional | Receiver's last name, e.g. Wayne |
| > rcvrCountry | String | Conditional | The recipient's country, e.g. United States<br>You must enter an English country name or a two letter country code (ISO 3166-1). Please refer to the Country Name and Country Code in the country information table below. |
| > rcvrCountrySubDivision | String | Conditional | State/Province of the recipient, e.g. California |
| > rcvrTownName | String | Conditional | The town/city where the recipient is located, e.g. San Jose |

| > rcvrStreetName | String | Conditional |	Recipient's street address, e.g. Clementi Avenue 1 |
| clientId |	String |	No |	Client-supplied ID A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |


# Response Example

```
{
    "code": "0",
    "msg": "",
    "data": [{
        "amt": "0.1",
        "wdId": "67485",
        "ccy": "BTC",
        "clientId": "",
        "chain": "BTC-Bitcoin"
    }]
}
```

## Response Parameters

| Parameter  | Type   | Description |
|------------|--------|-------------|
| ccy        | String | Currency |
| chain      | String | Chain name, e.g. USDT-ERC20, USDT-TRC20 |
| amt        | String | Withdrawal amount |
| wdId       | String | Withdrawal ID |
| clientId   | String | Client-supplied ID. A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |

## Country Information

| Country Name                  | Country Code |
|-------------------------------|--------------|
| Afghanistan                   | AF           |
| Albania                       | AL           |
| Algeria                       | DZ           |
| .....                         |              |

## Cancel Withdrawal

You can cancel normal withdrawal requests, but you cannot cancel withdrawal requests on Lightning.

**Rate Limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

### HTTP Request
`POST /api/v5/asset/cancel-withdrawal`

### Request Example
```
POST /api/v5/asset/cancel-withdrawal
body {
   "wdId":"1123456"
}
```

### Request Parameters

| Parameter | Type   | Required | Description     |
|-----------|--------|----------|-----------------|
| wdId      | String | Yes      | Withdrawal ID   |

### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "wdId": "1123456"   
    }
  ]
}
```

### Response Parameters

| Parameter | Type   | Description     |
|-----------|--------|-----------------|
| wdId      | String | Withdrawal ID   |

> Note: If the code is equal to 0, it cannot be strictly considered that the withdrawal has been revoked. It only means that your request is accepted by the server. The actual result is subject to the status in the withdrawal history.

## Get Withdrawal History

Retrieve the withdrawal records according to the currency, withdrawal status, and time range in reverse chronological order. The 100 most recent records are returned by default.

Websocket API is also available, refer to Withdrawal info channel.

**Rate Limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`GET /api/v5/asset/withdrawal-history`

### Request Example

```
GET /api/v5/asset/withdrawal-history

# Query withdrawal history from 2022-06-01 to 2022-07-01
GET /api/v5/asset/withdrawal-history?ccy=BTC&after=1654041600000&before=1656633600000
```

### Request Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| ccy       | String | No       | Currency, e.g. BTC |
| wdId      | String | No       | Withdrawal ID |
| clientId  | String | No       | Client-supplied ID. A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| txId      | String | No       | Hash record of the deposit |
| type      | String | No       | Withdrawal type. 3: Internal transfer, 4: On-chain withdrawal |
| state     | String | No       | Status of withdrawal (see full status list in original) |
| after     | String | No       | Pagination of data to return records earlier than the requested ts, Unix timestamp format in milliseconds |
| before    | String | No       | Pagination of data to return records newer than the requested ts, Unix timestamp format in milliseconds |
| limit     | String | No       | Number of results per request. The maximum is 100; The default is 100 |

### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "note": "",
      "chain": "ETH-Ethereum",
      "fee": "0.007",
      "feeCcy": "ETH",
      "ccy": "ETH",
      "clientId": "",
      "toAddrType": "1",
      "amt": "0.029809",
      "txId": "0x35c******b360a174d",
      "from": "156****359",
      "areaCodeFrom": "86",
      "to": "0xa30d1fab********7CF18C7B6C579",
      "areaCodeTo": "",
      "state": "2",
      "ts": "1655251200000",
      "nonTradableAsset": false,
      "wdId": "15447421"
    }
  ]
}
```

### Response Parameters

| Parameter         | Type    | Description |
|-------------------|---------|-------------|
| ccy              | String  | Currency |
| chain            | String  | Chain name, e.g. USDT-ERC20, USDT-TRC20 |
| nonTradableAsset | Boolean | Whether it is a non-tradable asset or not. true: non-tradable asset, false: tradable asset |
| amt              | String  | Withdrawal amount |
| ts               | String  | Time the withdrawal request was submitted, Unix timestamp format in milliseconds, e.g. 1655251200000 |
| from             | String  | Withdrawal account. It can be email/phone/sub-account name |
| areaCodeFrom     | String  | Area code for the phone number. If from is a phone number, this parameter returns the area code for the phone number |
| to               | String  | Receiving address |
| areaCodeTo       | String  | Area code for the phone number. If to is a phone number, this parameter returns the area code for the phone number |
| toAddrType       | String  | Address type. 1: wallet address, email, phone, or login account name. 2: UID |
| tag              | String  | Some currencies require a tag for withdrawals. This is not returned if not required. |
| pmtId            | String  | Some currencies require a payment ID for withdrawals. This is not returned if not required. |
| memo             | String  | Some currencies require this parameter for withdrawals. This is not returned if not required. |
| addrEx           | Object  | Withdrawal address attachment (This will not be returned if the currency does not require this) e.g. TONCOIN attached tag name is comment, the return will be {'comment':'123456'} |
| txId             | String  | Hash record of the withdrawal. This parameter will return "" for internal transfers. |
| fee              | String  | Withdrawal fee amount |
| feeCcy           | String  | Withdrawal fee currency, e.g. USDT |
| state            | String  | Status of withdrawal |
| wdId             | String  | Withdrawal ID |
| clientId         | String  | Client-supplied ID |
| note             | String  | Withdrawal note |

## Get Deposit Withdraw Status

Retrieve deposit's and withdrawal's detailed status and estimated complete time.

**Rate Limit**: 1 request per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`GET /api/v5/asset/deposit-withdraw-status`

### Request Example

```
# For deposit
GET /api/v5/asset/deposit-withdraw-status?txId=xxxxxx&to=1672734730284&ccy=USDT&chain=USDT-ERC20

# For withdrawal
GET /api/v5/asset/deposit-withdraw-status?wdId=200045249
```

### Request Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| wdId      | String | Conditional | Withdrawal ID, use to retrieve withdrawal status. Required to input one and only one of wdId and txId |
| txId      | String | Conditional | Hash record of the deposit, use to retrieve deposit status. Required to input one and only one of wdId and txId |
| ccy       | String | Conditional | Currency type, e.g. USDT. Required when retrieving deposit status with txId |
| to        | String | Conditional | To address, the destination address in deposit. Required when retrieving deposit status with txId |
| chain     | String | Conditional | Currency chain information, e.g. USDT-ERC20. Required when retrieving deposit status with txId |

### Response Example
```
{
    "code":"0",
    "data":[
        {
            "wdId": "200045249",
            "txId": "16f3638329xxxxxx42d988f97", 
            "state": "Pending withdrawal: Wallet is under maintenance, please wait.",
            "estCompleteTime": "01/09/2023, 8:10:48 PM"
        }
    ],
    "msg": ""
}
```

### Response Parameters

| Parameter        | Type   | Description |
|------------------|--------|-------------|
| estCompleteTime  | String | Estimated complete time. The timezone is UTC+8. The format is MM/dd/yyyy, h:mm:ss AM/PM. estCompleteTime is only an approximate estimated time, for reference only. |
| state            | String | The detailed stage and status of the deposit/withdrawal. The message in front of the colon is the stage; the message after the colon is the ongoing status. |
| txId             | String | Hash record on-chain. For withdrawal, if the txId has already been generated, it will return the value, otherwise, it will return "". |
| wdId             | String | Withdrawal ID. When retrieving deposit status, wdId returns blank "". |

### Stage References

**Deposit**  
Stage 1: On-chain transaction detection  
Stage 2: Push deposit data to associated account  
Stage 3: Receiving account credit  
Final stage: Deposit complete  

**Withdrawal**  
Stage 1: Pending withdrawal  
Stage 2: Withdrawal in progress  
Final stage: Withdrawal complete / cancellation complete  

## Get Exchange List (Public)

Authentication is not required for this public endpoint.

**Rate Limit**: 6 requests per second  
**Rate limit rule**: IP  

### HTTP Request
`GET /api/v5/asset/exchange-list`

### Request Example
```
GET /api/v5/asset/exchange-list
```

### Request Parameters
None

### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "exchId": "did:ethr:0xfeb4f99829a9acdf52979abee87e83addf22a7e1",
        "exchName": "1xbet"
    }
  ]
}
```

### Response Parameters

| Parameter | Type   | Description |
|-----------|--------|-------------|
| exchName  | String | Exchange name, e.g. 1xbet |
| exchId    | String | Exchange ID, e.g. did:ethr:0xfeb4f99829a9acdf52979abee87e83addf22a7e1 |
```markdown
## Apply for Monthly Statement (Last Year)

Apply for monthly statement in the past year.

**Rate Limit**: 20 requests per month  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`POST /api/v5/asset/monthly-statement`

### Request Example
```
POST /api/v5/asset/monthly-statement
body
{
    "month":"Jan"
}
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| month | String | No | Month, last month by default. Valid values: Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "ts": "1646892328000"
        }
    ],
    "msg": ""
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ts | String | Download link generation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |

## Get Monthly Statement (Last Year)

Retrieve monthly statement in the past year.

**Rate Limit**: 10 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`GET /api/v5/asset/monthly-statement`

### Request Example
```
GET /api/v5/asset/monthly-statement?month=Jan
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| month | String | Yes | Month, valid values: Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "fileHref": "http://xxx",
            "state": "finished",
            "ts": 1646892328000
        }
    ],
    "msg": ""
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| fileHref | String | Download file link |
| ts | Int | Download link generation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| state | String | Download link status: "finished", "ongoing" |

## Get Convert Currencies

**Rate Limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`GET /api/v5/asset/convert/currencies`

### Request Example
```
GET /api/v5/asset/convert/currencies
```

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "min": "",  // Deprecated
            "max": "",  // Deprecated
            "ccy": "BTC"
        },
        {
            "min": "",
            "max": "",
            "ccy": "ETH"
        }
    ],
    "msg": ""
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency, e.g. BTC |
| min | String | Minimum amount to convert (Deprecated) |
| max | String | Maximum amount to convert (Deprecated) |

## Get Convert Currency Pair

**Rate Limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`GET /api/v5/asset/convert/currency-pair`

### Request Example
```
GET /api/v5/asset/convert/currency-pair?fromCcy=USDT&toCcy=BTC
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| fromCcy | String | Yes | Currency to convert from, e.g. USDT |
| toCcy | String | Yes | Currency to convert to, e.g. BTC |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "baseCcy": "BTC",
            "baseCcyMax": "0.5",
            "baseCcyMin": "0.0001",
            "instId": "BTC-USDT",
            "quoteCcy": "USDT",
            "quoteCcyMax": "10000",
            "quoteCcyMin": "1"
        }
    ],
    "msg": ""
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| instId | String | Currency pair, e.g. BTC-USDT |
| baseCcy | String | Base currency, e.g. BTC in BTC-USDT |
| baseCcyMax | String | Maximum amount of base currency |
| baseCcyMin | String | Minimum amount of base currency |
| quoteCcy | String | Quote currency, e.g. USDT in BTC-USDT |
| quoteCcyMax | String | Maximum amount of quote currency |
| quoteCcyMin | String | Minimum amount of quote currency |


### Estimate Quote

Get an estimate quote for currency conversion.

- **Rate Limit:** 10 requests per second
- **Rate Limit Rule:** User ID
- **Rate Limit:** 1 request per 5 seconds
- **Rate Limit Rule:** Instrument ID
- **Permission:** Trade

#### HTTP Request
```
POST /api/v5/asset/convert/estimate-quote
```

#### Request Example
```
POST /api/v5/asset/convert/estimate-quote
{
    "baseCcy": "ETH",
    "quoteCcy": "USDT",
    "side": "buy",
    "rfqSz": "30",
    "rfqSzCcy": "USDT"
}
```

#### Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| baseCcy    | String | Yes      | Base currency, e.g. BTC in BTC-USDT |
| quoteCcy   | String | Yes      | Quote currency, e.g. USDT in BTC-USDT |
| side       | String | Yes      | Trade side based on baseCcy (buy/sell) |
| rfqSz      | String | Yes      | RFQ amount |
| rfqSzCcy   | String | Yes      | RFQ currency |
| clQReqId   | String | No       | Client Order ID as assigned by the client. A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag        | String | No       | Order tag. Applicable to broker user |

#### Response Example
```
{
    "code": "0",
    "data": [
        {
            "baseCcy": "ETH",
            "baseSz": "0.01023052",
            "clQReqId": "",
            "cnvtPx": "2932.40104429",
            "origRfqSz": "30",
            "quoteCcy": "USDT",
            "quoteId": "quoterETH-USDT16461885104612381",
            "quoteSz": "30",
            "quoteTime": "1646188510461",
            "rfqSz": "30",
            "rfqSzCcy": "USDT",
            "side": "buy",
            "ttlMs": "10000"
        }
    ],
    "msg": ""
}
```

#### Response Parameters
| Parameter  | Type   | Description |
|------------|--------|-------------|
| quoteTime  | String | Quotation generation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| ttlMs      | String | Validity period of quotation in milliseconds |
| clQReqId   | String | Client Order ID as assigned by the client |
| quoteId    | String | Quote ID |
| baseCcy    | String | Base currency, e.g. BTC in BTC-USDT |
| quoteCcy   | String | Quote currency, e.g. USDT in BTC-USDT |
| side       | String | Trade side based on baseCcy |
| origRfqSz  | String | Original RFQ amount |
| rfqSz      | String | Real RFQ amount |
| rfqSzCcy   | String | RFQ currency |
| cnvtPx     | String | Convert price based on quote currency |
| baseSz     | String | Convert amount of base currency |
| quoteSz    | String | Convert amount of quote currency |

---

### Convert Trade

Execute a currency conversion trade. You should make estimate quote before convert trade.

> **Note:** Only assets in the trading account supported convert.

- **Rate Limit:** 10 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Trade
- **Trading Limit:** For the same side (buy/sell), there's a trading limit of 1 request per 5 seconds.

#### HTTP Request
```
POST /api/v5/asset/convert/trade
```

#### Request Example
```
POST /api/v5/asset/convert/trade
{
    "baseCcy": "ETH",
    "quoteCcy": "USDT",
    "side": "buy",
    "sz": "30",
    "szCcy": "USDT",
    "quoteId": "quoterETH-USDT16461885104612381"
}
```

#### Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| quoteId    | String | Yes      | Quote ID |
| baseCcy    | String | Yes      | Base currency, e.g. BTC in BTC-USDT |
| quoteCcy   | String | Yes      | Quote currency, e.g. USDT in BTC-USDT |
| side       | String | Yes      | Trade side based on baseCcy (buy/sell) |
| sz         | String | Yes      | Quote amount. The quote amount should no more then RFQ amount |
| szCcy      | String | Yes      | Quote currency |
| clTReqId   | String | No       | Client Order ID as assigned by the client. A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag        | String | No       | Order tag. Applicable to broker user |

#### Response Example
```
{
    "code": "0",
    "data": [
        {
            "baseCcy": "ETH",
            "clTReqId": "",
            "fillBaseSz": "0.01023052",
            "fillPx": "2932.40104429",
            "fillQuoteSz": "30",
            "instId": "ETH-USDT",
            "quoteCcy": "USDT",
            "quoteId": "quoterETH-USDT16461885104612381",
            "side": "buy",
            "state": "fullyFilled",
            "tradeId": "trader16461885203381437",
            "ts": "1646188520338"
        }
    ],
    "msg": ""
}
```

#### Response Parameters
| Parameter     | Type   | Description |
|---------------|--------|-------------|
| tradeId       | String | Trade ID |
| quoteId       | String | Quote ID |
| clTReqId      | String | Client Order ID as assigned by the client |
| state         | String | Trade state (fullyFilled: success, rejected: failed) |
| instId        | String | Currency pair, e.g. BTC-USDT |
| baseCcy       | String | Base currency, e.g. BTC in BTC-USDT |
| quoteCcy      | String | Quote currency, e.g. USDT in BTC-USDT |
| side          | String | Trade side based on baseCcy (buy/sell) |
| fillPx        | String | Filled price based on quote currency |
| fillBaseSz    | String | Filled amount for base currency |
| fillQuoteSz   | String | Filled amount for quote currency |
| ts            | String | Convert trade time, Unix timestamp format in milliseconds, e.g. 1597026383085 |



### Get Convert History

Retrieve conversion trade history.

- **Rate Limit:** 6 requests per second
- **Rate Limit Rule:** User ID

#### HTTP Request
```
GET /api/v5/asset/convert/history
```

#### Request Example
```
GET /api/v5/asset/convert/history
```

#### Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| clTReqId   | String | No       | Client Order ID as assigned by the client. A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| after      | String | No       | Pagination of data to return records earlier than the requested ts, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| before     | String | No       | Pagination of data to return records newer than the requested ts, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| limit      | String | No       | Number of results per request. The maximum is 100. The default is 100. |
| tag        | String | No       | Order tag. Applicable to broker user. If the convert trading used tag, this parameter is also required. |

#### Response Example
```
{
    "code": "0",
    "data": [
        {
            "clTReqId": "",
            "instId": "ETH-USDT",
            "side": "buy",
            "fillPx": "2932.401044",
            "baseCcy": "ETH",
            "quoteCcy": "USDT",
            "fillBaseSz": "0.01023052",
            "state": "fullyFilled",
            "tradeId": "trader16461885203381437",
            "fillQuoteSz": "30",
            "ts": "1646188520000"
        }
    ],
    "msg": ""
}
```

#### Response Parameters
| Parameter     | Type   | Description |
|---------------|--------|-------------|
| tradeId       | String | Trade ID |
| clTReqId      | String | Client Order ID as assigned by the client |
| state         | String | Trade state (fullyFilled: success, rejected: failed) |
| instId        | String | Currency pair, e.g. BTC-USDT |
| baseCcy       | String | Base currency, e.g. BTC in BTC-USDT |
| quoteCcy      | String | Quote currency, e.g. USDT in BTC-USDT |
| side          | String | Trade side based on baseCcy (buy/sell) |
| fillPx        | String | Filled price based on quote currency |
| fillBaseSz    | String | Filled amount for base currency |
| fillQuoteSz   | String | Filled amount for quote currency |
| ts            | String | Convert trade time, Unix timestamp format in milliseconds, e.g. 1597026383085 |

---

## Fiat APIs

### Get Deposit Payment Methods

Display all available fiat deposit payment methods.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Read

#### HTTP Request
```
GET /api/v5/fiat/deposit-payment-methods
```

#### Request Example
```
GET /api/v5/fiat/deposit-payment-methods?ccy=TRY
{
  "ccy" : "TRY"
}
```

#### Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| ccy        | String | Yes      | Fiat currency, ISO-4217 3 digit currency code, e.g. TRY |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "ccy": "TRY",
      "paymentMethod": "TR_BANKS",
      "feeRate": "0",
      "minFee": "0",
      "limits": {
        "dailyLimit": "2147483647",
        "dailyLimitRemaining": "2147483647",
        "weeklyLimit": "2147483647",
        "weeklyLimitRemaining": "2147483647",
        "monthlyLimit": "",
        "monthlyLimitRemaining": "",
        "maxAmt": "1000000",
        "minAmt": "1",
        "lifetimeLimit": "2147483647"
      },
      "accounts": [
          {
            "paymentAcctId": "1",
            "acctNum": "TR740001592093703829602611",
            "recipientName": "John Doe",
            "bankName": "VakıfBank",
            "bankCode": "TVBATR2AXXX",
            "state": "active"
          },
          {
            "paymentAcctId": "2",
            "acctNum": "TR740001592093703829602622",
            "recipientName": "John Doe",
            "bankName": "FBHLTRISXXX",
            "bankCode": "",
            "state": "active"
          }
      ]
    }
  ]
}
```

#### Response Parameters
| Parameter                   | Type             | Description |
|----------------------------|------------------|-------------|
| ccy                        | String           | Fiat currency |
| paymentMethod              | String           | The payment method associated with the currency (TR_BANKS, PIX, SEPA) |
| feeRate                    | String           | The fee rate for each deposit, expressed as a percentage. e.g. 0.02 represents 2 percent fee for each transaction. |
| minFee                     | String           | The minimum fee for each deposit |
| limits                     | Object           | An object containing limits for various transaction intervals |
| > dailyLimit               | String           | The daily transaction limit |
| > dailyLimitRemaining      | String           | The remaining daily transaction limit |
| > weeklyLimit              | String           | The weekly transaction limit |
| > weeklyLimitRemaining     | String           | The remaining weekly transaction limit |
| > monthlyLimit             | String           | The monthly transaction limit |
| > monthlyLimitRemaining    | String           | The remaining monthly transaction limit |
| > maxAmt                   | String           | The maximum amount allowed per transaction |
| > minAmt                   | String           | The minimum amount allowed per transaction |
| > lifetimeLimit            | String           | The lifetime transaction limit. Return the configured value, "" if not configured |
| accounts                   | Array of Object  | An array containing information about payment accounts associated with the currency and method |
| > paymentAcctId            | String           | The account ID for withdrawal |
| > acctNum                  | String           | The account number, which can be an IBAN or other bank account number |
| > recipientName            | String           | The name of the recipient |
| > bankName                 | String           | The name of the bank associated with the account |
| > bankCode                 | String           | The SWIFT code / BIC / bank code associated with the account |
| > state                    | String           | The state of the account (active) |

---

### Get Withdrawal Payment Methods

Display all available fiat withdrawal payment methods.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Read

#### HTTP Request
```
GET /api/v5/fiat/withdrawal-payment-methods
```

#### Request Example
```
GET /api/v5/fiat/withdrawal-payment-methods?ccy=TRY
```

#### Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| ccy        | String | Yes      | Fiat currency, ISO-4217 3 digit currency code. e.g. TRY |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "ccy": "TRY",
      "paymentMethod": "TR_BANKS",
      "feeRate": "0.02",
      "minFee": "1",
      "limits": {
        "dailyLimit": "",
        "dailyLimitRemaining": "",
        "weeklyLimit": "",
        "weeklyLimitRemaining": "",
        "monthlyLimit": "",
        "monthlyLimitRemaining": "",
        "maxAmt": "",
        "minAmt": "",
        "lifetimeLimit": ""
      },
      "accounts": [
          {
            "paymentAcctId": "1",
            "acctNum": "TR740001592093703829602668",
            "recipientName": "John Doe",
            "bankName": "VakıfBank",
            "bankCode": "TVBATR2AXXX",
            "state": "active"
          },
          {
            "paymentAcctId": "2",
            "acctNum": "TR740001592093703829603024",
            "recipientName": "John Doe",
            "bankName": "Şekerbank",
            "bankCode": "SEKETR2AXXX",
            "state": "active"
          }
      ]
    }
  ]
}
```

#### Response Parameters
| Parameter                   | Type             | Description |
|----------------------------|------------------|-------------|
| ccy                        | String           | Fiat currency |
| paymentMethod              | String           | The payment method associated with the currency (TR_BANKS, PIX, SEPA) |
| feeRate                    | String           | The fee rate for each deposit, expressed as a percentage. e.g. 0.02 represents 2 percent fee for each transaction. |
| minFee                     | String           | The minimum fee for each deposit |
| limits                     | Object           | An object containing limits for various transaction intervals |
| > dailyLimit               | String           | The daily transaction limit |
| > dailyLimitRemaining      | String           | The remaining daily transaction limit |
| > weeklyLimit              | String           | The weekly transaction limit |
| > weeklyLimitRemaining     | String           | The remaining weekly transaction limit |
| > monthlyLimit             | String           | The monthly transaction limit |
| > monthlyLimitRemaining    | String           | The remaining monthly transaction limit |
| > minAmt                   | String           | The minimum amount allowed per transaction |
| > maxAmt                   | String           | The maximum amount allowed per transaction |
| > lifetimeLimit            | String           | The lifetime transaction limit. Return the configured value, "" if not configured |
| accounts                   | Array of Object  | An array containing information about payment accounts associated with the currency and method |
| > paymentAcctId            | String           | The account ID for withdrawal |
| > acctNum                  | String           | The account number, which can be an IBAN or other bank account number |
| > recipientName            | String           | The name of the recipient |
| > bankName                 | String           | The name of the bank associated with the account |
| > bankCode                 | String           | The SWIFT code / BIC / bank code associated with the account |
| > state                    | String           | The state of the account (active) |

---

### Create Withdrawal Order

Initiate a fiat withdrawal request (Authenticated endpoint, Only for API keys with "Withdrawal" access).

> **Note:** Only supported withdrawal of assets from funding account.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Withdraw

#### HTTP Request
```
POST /api/v5/fiat/create-withdrawal
```

#### Request Example
```
POST /api/v5/fiat/create-withdrawal
{
    "paymentAcctId": "412323",
    "ccy": "TRY",
    "amt": "10000",
    "paymentMethod": "TR_BANKS",
    "clientId": "194a6975e98246538faeb0fab0d502df"
}
```

#### Request Parameters
| Parameters     | Type   | Required | Description |
|----------------|--------|----------|-------------|
| paymentAcctId  | String | Yes      | Payment account id to withdraw to, retrieved from get withdrawal payment methods API |
| ccy            | String | Yes      | Currency for withdrawal, must match currency allowed for paymentMethod |
| amt            | String | Yes      | Requested withdrawal amount before fees. Has to be less than or equal to 2 decimal points double |
| paymentMethod  | String | Yes      | Payment method to use for withdrawal (TR_BANKS, PIX, SEPA) |
| clientId       | String | Yes      | Client-supplied ID, A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. e.g. 194a6975e98246538faeb0fab0d502df |
| clTReqId       | String | No       | Client Order ID as assigned by the client. A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag            | String | No       | Order tag. Applicable to broker user |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "024041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "100",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": ""
    }
  ]
}
```

#### Response Parameters
| Parameter      | Type   | Description |
|----------------|--------|-------------|
| ordId          | String | Order ID |
| clientId       | String | The original request ID associated with the transaction. If it's a deposit, it's most likely an empty string (""). |
| amt            | String | Amount of the transaction |
| ccy            | String | The currency of the transaction |
| fee            | String | The transaction fee |
| paymentAcctId  | String | The ID of the payment account used |
| paymentMethod  | String | Payment method, e.g. TR_BANKS |
| state          | String | The state of the transaction (completed, failed, pending, canceled, inqueue, processing) |
| cTime          | String | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| uTime          | String | The update time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
        "uTime": "1707429385000",
        "ordId": "124041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "fee": "0",
        "amt": "100",
        "ccy": "TRY",
        "state": "completed",
        "clientId": "194a6975e98246538faeb0fab0d502df"
    }
  ]
}
```

#### Response Parameters
| Parameter      | Type   | Description |
|----------------|--------|-------------|
| ordId          | String | The unique order Id |
| clientId       | String | The client ID associated with the transaction |
| amt            | String | The requested amount for the transaction |
| ccy            | String | The currency of the transaction |
| fee            | String | The transaction fee |
| paymentAcctId  | String | The Id of the payment account used |
| paymentMethod  | String | Payment Method (TR_BANKS, PIX, SEPA) |
| state          | String | The State of the transaction (processing, completed) |
| cTime          | String | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| uTime          | String | The update time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |

---

### Cancel Withdrawal Order

Cancel a pending fiat withdrawal order, currently only applicable to TRY.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Trade

#### HTTP Request
```
POST /api/v5/fiat/cancel-withdrawal
```

#### Request Example
```
POST /api/v5/fiat/cancel-withdrawal
{
    "ordId": "124041201450544699"
}
```

#### Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| ordId      | String | Yes      | Payment Order Id |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "ordId": "124041201450544699",
        "state": "canceled"
    }
  ]
}
```

#### Response Parameters
| Parameter | Type   | Description |
|-----------|--------|-------------|
| ordId     | String | Payment Order ID |
| state     | String | The state of the transaction, e.g. canceled |

---

### Get Withdrawal Order History

Get fiat withdrawal order history.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Read

#### HTTP Request
```
GET /api/v5/fiat/withdrawal-order-history
```

#### Request Example
```
GET /api/v5/fiat/withdrawal-order-history
```

#### Request Parameters
| Parameters    | Types  | Required | Description |
|---------------|--------|----------|-------------|
| ccy           | String | No       | Fiat currency, ISO-4217 3 digit currency code, e.g. TRY |
| paymentMethod | String | No       | Payment Method (TR_BANKS, PIX, SEPA) |
| state         | String | No       | State of the order (completed, failed, pending, canceled, inqueue, processing) |
| after         | String | No       | Filter with a begin timestamp. Unix timestamp format in milliseconds (inclusive), e.g. 1597026383085 |
| before        | String | No       | Filter with an end timestamp. Unix timestamp format in milliseconds (inclusive), e.g. 1597026383085 |
| limit         | String | No       | Number of results per request. Maximum and default is 100 |
| tag           | String | No       | Order tag. Applicable to broker user. If the convert trading used tag, this parameter is also required. |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "124041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "10000",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": "194a6975e98246538faeb0fab0d502df"
    },
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "124041201450544690",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "5000",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": "164a6975e48946538faeb0fab0d414fg"
    }
  ]
}
```

#### Response Parameters
| Parameter      | Type   | Description |
|----------------|--------|-------------|
| ordId          | String | Unique Order Id |
| clientId       | String | Client Id of the transaction |
| amt            | String | Final amount of the transaction |
| ccy            | String | Currency of the transaction |
| fee            | String | Transaction fee |
| paymentAcctId  | String | ID of the payment account used |
| paymentMethod  | String | Payment method type |
| state          | String | State of the transaction (completed, failed, pending, canceled, inqueue, processing) |
| cTime          | String | Creation time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| uTime          | String | Update time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |

---

### Get Withdrawal Order Detail

Get fiat withdraw order detail.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Read

#### HTTP Request
```
GET /api/v5/fiat/withdrawal
```

#### Request Example
```
GET /api/v5/fiat/withdrawal?ordId=024041201450544699
{
    "ordId": "024041201450544699"
}
```

#### Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| ordId      | String | Yes      | Order ID |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "024041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "100",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": "194a6975e98246538faeb0fab0d502df"
    }
  ]
}
```

#### Response Parameters
| Parameter      | Type   | Description |
|----------------|--------|-------------|
| ordId          | String | Order ID |
| clientId       | String | The original request ID associated with the transaction |
| ccy            | String | The currency of the transaction |
| amt            | String | Amount of the transaction |
| fee            | String | The transaction fee |
| paymentAcctId  | String | The ID of the payment account used |
| paymentMethod  | String | Payment method, e.g. TR_BANKS |
| state          | String | The state of the transaction (completed, failed, pending, canceled, inqueue, processing) |
| cTime          | String | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| uTime          | String | The update time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |

---

### Get Deposit Order History

Get fiat deposit order history.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Read

#### HTTP Request
```
GET /api/v5/fiat/deposit-order-history
```

#### Request Example
```
GET /api/v5/fiat/deposit-order-history
```

#### Request Parameters
| Parameters    | Types  | Required | Description |
|---------------|--------|----------|-------------|
| ccy           | String | No       | ISO-4217 3 digit currency code |
| paymentMethod | String | No       | Payment Method (TR_BANKS, PIX, SEPA) |
| state         | String | No       | State of the order (completed, failed, pending, canceled, inqueue, processing) |
| after         | String | No       | Filter with a begin timestamp. Unix timestamp format in milliseconds (inclusive), e.g. 1597026383085 |
| before        | String | No       | Filter with an end timestamp. Unix timestamp format in milliseconds (inclusive), e.g. 1597026383085 |
| limit         | String | No       | Number of results per request. Maximum and default is 100 |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "024041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "10000",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": ""
    },
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "024041201450544690",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "50000",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": ""
    }
  ]
}
```

#### Response Parameters
| Parameter      | Type   | Description |
|----------------|--------|-------------|
| ordId          | String | Unique Order ID |
| clientId       | String | Client Id of the transaction |
| ccy            | String | Currency of the transaction |
| amt            | String | Final amount of the transaction |
| fee            | String | Transaction fee |
| paymentAcctId  | String | ID of the payment account used |
| paymentMethod  | String | Payment Method, e.g. TR_BANKS |
| state          | String | State of the transaction (completed, failed, pending, canceled, inqueue) |
| cTime          | String | Creation time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| uTime          | String | Update time of the transaction, Unix timestamp format in milliseconds, e.g. 1597026383085 |

---

### Get Deposit Order Detail

Get fiat deposit order detail.

- **Rate Limit:** 3 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Read

#### HTTP Request
```
GET /api/v5/fiat/deposit
```

#### Request Example
```
GET /api/v5/fiat/deposit?ordId=024041201450544699
{
    "ordId": "024041201450544699"
}
```

#### Request Parameters
| Parameters | Types  | Required | Description |
|------------|--------|----------|-------------|
| ordId      | String | Yes      | Order ID |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "024041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "100",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": ""
    }
  ]
}
```

# Response Parameters

| Parameter        | Type   | Description                                                                                   |
|------------------|--------|-----------------------------------------------------------------------------------------------|
| ordId            | String | Order ID                                                                                      |
| clientId         | String | The original request ID associated with the transaction. For deposits, likely an empty string ("") |
| amt              | String | Amount of the transaction                                                                     |
| ccy              | String | The currency of the transaction                                                              |
| fee              | String | The transaction fee                                                                          |
| paymentAcctId    | String | The ID of the payment account used                                                           |
| paymentMethod    | String | Payment method, e.g. `TR_BANKS`                                                              |
| state            | String | The state of the transaction: <br>`completed` <br>`failed` <br>`pending` <br>`canceled` <br>`inqueue` <br>`processing` |
| cTime            | String | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| uTime            | String | The update time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085`  |




## WebSocket

### Deposit Info Channel

A push notification is triggered when a deposit is initiated or the deposit status changes.

**URL Path**: `/ws/v5/business` (required login)

#### Request Example
```
{
    "id": "1512",
    "op": "subscribe",
    "args": [
        {
            "channel": "deposit-info"
        }
    ]
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message |
| op | String | Yes | Operation: subscribe, unsubscribe |
| args | Array | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name: deposit-info |
| > ccy | String | No | Currency, e.g. BTC |

#### Response Example
```
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "deposit-info"
    },
    "connId": "a4d3ae55"
}
```

# Failure Response Example

## Response parameters

| Parameter   | Type   | Required | Description                     |
|-------------|--------|----------|---------------------------------|
| id          | String | No       | Unique identifier of the message |
| event       | String | Yes      | Operation: subscribe / unsubscribe / error |
| arg         | Object | No       | Subscribed channel              |
| > channel   | String | Yes      | Channel name (e.g. deposit-info) |
| > ccy       | String | No       | Currency, e.g. BTC              |
| code        | String | No       | Error code                     |
| msg         | String | No       | Error message                  |
| connId      | String | Yes      | WebSocket connection ID        |

---

# Push Data Example

## Push data parameters

| Parameter       | Type        | Description                                                                                     |
|-----------------|-------------|-------------------------------------------------------------------------------------------------|
| arg             | Object      | Successfully subscribed channel                                                                 |
| > channel       | String      | Channel name (e.g. deposit-info)                                                                |
| > uid           | String      | User Identifier                                                                                 |
| > ccy           | String      | Currency, e.g. BTC                                                                             |
| data            | Array       | Array of objects, subscribed data                                                               |

### Subscribed data parameters

| Parameter             | Type   | Description                                                                                          |
|-----------------------|--------|----------------------------------------------------------------------------------------------------|
| > uid                 | String | User Identifier of the message producer                                                            |
| > subAcct             | String | Sub-account name. If the message producer is master account, returns ""                            |
| > pTime               | String | Push time, the millisecond format of the Unix timestamp, e.g. 1597026383085                        |
| > ccy                 | String | Currency                                                                                           |
| > chain               | String | Chain name                                                                                         |
| > amt                 | String | Deposit amount                                                                                    |
| > from                | String | Deposit account. Only internal OKX account is returned, not blockchain address                     |
| > areaCodeFrom        | String | If from is a phone number, this returns area code of the phone number                             |
| > to                  | String | Deposit address                                                                                   |
| > txId                | String | Hash record of the deposit                                                                       |
| > ts                  | String | Time deposit record is created, Unix timestamp in milliseconds, e.g. 1655251200000                |
| > state               | String | Status of deposit: <br>0: waiting for confirmation <br>1: deposit credited <br>2: deposit successful <br>8: pending due to temporary deposit suspension on this cryptocurrency <br>11: match the address blacklist <br>12: account or deposit is frozen <br>13: sub-account deposit interception <br>14: KYC limit |
| > depId               | String | Deposit ID                                                                                       |
| > fromWdId            | String | Internal transfer initiator's withdrawal ID. If deposit is from internal transfer, else ""       |
| > actualDepBlkConfirm | String | Actual amount of blockchain confirmed in a single deposit                                        |

---

# Withdrawal info channel

- A push notification is triggered when a withdrawal is initiated or withdrawal status changes.
- Supports subscriptions for accounts.
- Master account subscription receives push for master and sub-account withdrawals.
- Sub-account subscription receives only sub-account withdrawal info.

### URL Path

`/ws/v5/business` (required login)

---

## Request Example

### Request Parameters

| Parameter | Type         | Required | Description                                                                              |
|-----------|--------------|----------|------------------------------------------------------------------------------------------|
| id        | String       | No       | Unique identifier of the message. Provided by client. Returned in response for tracking. |
| op        | String       | Yes      | Operation: subscribe / unsubscribe                                                     |
| args      | Array<Object> | Yes      | List of subscribed channels                                                             |

#### Args object details

| Parameter | Type   | Required | Description         |
|-----------|--------|----------|---------------------|
| channel   | String | Yes      | Channel name: withdrawal-info |
| ccy       | String | No       | Currency, e.g. BTC   |

---

### Response parameters

| Parameter | Type   | Required | Description                      |
|-----------|--------|----------|---------------------------------|
| id        | String | No       | Unique identifier of the message |
| event     | String | Yes      | Operation: subscribe / unsubscribe / error |
| arg       | Object | No       | Subscribed channel               |
| > channel | String | Yes      | Channel name (withdrawal-info)  |
| > ccy     | String | No       | Currency, e.g. BTC              |
| code      | String | No       | Error code                     |
| msg       | String | No       | Error message                  |
| connId    | String | Yes      | WebSocket connection ID        |

---

# Push Data Example (Withdrawal info)

## Push data parameters

| Parameter | Type   | Description                     |
|-----------|--------|--------------------------------|
| arg       | Object | Successfully subscribed channel |
| > channel | String | Channel name                   |
| > uid     | String | User Identifier                |
| > ccy     | String | Currency, e.g. BTC             |
| data      | Array  | Array of subscribed data       |

### Subscribed data parameters

| Parameter         | Type   | Description                                                                                    |
|-------------------|--------|------------------------------------------------------------------------------------------------|
| > uid             | String | User Identifier of the message producer                                                       |
| > subAcct         | String | Sub-account name. Empty if master account                                                     |
| > pTime           | String | Push time, Unix timestamp in milliseconds, e.g. 1597026383085                                |
| > ccy             | String | Currency                                                                                      |
| > chain           | String | Chain name, e.g. USDT-ERC20, USDT-TRC20                                                     |
| > nonTradableAsset| String | Whether non-tradable asset: true / false                                                     |
| > amt             | String | Withdrawal amount                                                                            |
| > ts              | String | Time withdrawal request submitted, Unix timestamp in milliseconds, e.g. 1655251200000       |
| > from            | String | Withdrawal account (email, phone, sub-account name)                                          |
| > areaCodeFrom    | String | Area code if from is phone number                                                           |
| > to              | String | Receiving address                                                                           |
| > areaCodeTo      | String | Area code if to is phone number                                                             |
| > toAddrType      | String | Address type: <br>1: wallet address, email, phone, login account name <br>2: UID             |
| > tag             | String | Required tag for some currencies                                                             |
| > pmtId           | String | Payment ID required for some currencies                                                      |
| > memo            | String | Memo required for some currencies                                                           |
| > addrEx          | Object | Withdrawal address attachment (e.g. TONCOIN comment tag: {'comment':'123456'})               |
| > txId            | String | Hash record of withdrawal. "" for internal transfers                                         |
| > fee             | String | Withdrawal fee amount                                                                       |
| > feeCcy          | String | Withdrawal fee currency, e.g. USDT                                                          |
| > state           | String | Withdrawal status: <br>Stage 1: Pending withdrawal <br>17: Pending Travel Rule vendor response <br>10: Waiting transfer <br>0: Waiting withdrawal <br>4/5/6/8/9/12: Waiting manual review <br>7: Approved <br>Stage 2: Withdrawal in progress (only on-chain, no internal transfers) <br>1: Broadcasting transaction <br>15: Pending transaction validation <br>16: Delays up to 24h due to laws <br>-3: Canceling <br>Final stage: <br>-2: Canceled <br>-1: Failed <br>2: Success |
| > wdId            | String | Withdrawal ID                                                                             |
| > clientId        | String | Client-supplied ID                                                                        |
| > note            | String | Withdrawal note                                                                          |

## Sub-account

The API endpoints of sub-account require authentication.

REST API

### Get Sub-account List

Applies to master accounts only.

**Rate Limit**: 2 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

#### HTTP Request
`GET /api/v5/users/subaccount/list`

#### Request Example
```
GET /api/v5/users/subaccount/list
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| enable | String | No | Sub-account status: true (Normal), false (Frozen) |
| subAcct | String | No | Sub-account name |
| after | String | No | Query data earlier than requested timestamp (Unix ms) |
| before | String | No | Query data newer than requested timestamp (Unix ms) |
| limit | String | No | Number of results (max 100, default 100) |

#### Response Example
```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "canTransOut": false,
            "enable": true,
            "frozenFunc": [],
            "gAuth": false,
            "label": "D456DDDLx",
            "mobile": "",
            "subAcct": "D456DDDL",
            "ts": "1659334756000",
            "type": "1",
            "uid": "3400***********7413",
            "subAcctLv": "1",
            "firstLvSubAcct": "D456DDDL",
            "ifDma": false
        }
    ]
}
```

### Create Sub-account
Applies to master accounts only and master accounts API Key must be linked to IP addresses.

**Rate Limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

#### HTTP Request
`POST /api/v5/users/subaccount/create-subaccount`

#### Request Example
```
POST /api/v5/users/subaccount/create-subaccount
body
{
    "subAcct": "subAccount002",
    "type": "1",
    "label": "123456"
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| type | String | Yes | Sub-account type: 1 (Standard), 5 (Custody - Copper), 12 (Custody - Komainu) |
| label | String | No | Sub-account notes (6-32 characters) |
| pwd | String | Conditional | Sub-account login password (required for KYB users) |

#### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "label": "123456 ",
            "subAcct": "subAccount002",
            "ts": "1744875304520",
            "uid": "698827017768230914"
        }
    ]
}
```

### Create API Key for Sub-account
Applies to master accounts only.

**Rate Limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

#### HTTP Request
`POST /api/v5/users/subaccount/apikey`

#### Request Example
```
POST /api/v5/users/subaccount/apikey
body
{
    "subAcct":"panpanBroker2",
    "label":"broker3",
    "passphrase": "******",
    "perm":"trade"
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name (6-20 alphanumeric chars) |
| label | String | Yes | API Key note |
| passphrase | String | Yes | API Key password (8-32 chars with complexity) |
| perm | String | No | API Key permissions: read_only or trade |
| ip | String | No | Linked IP addresses (max 20) |

#### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "subAcct": "test-1",
        "label": "v5",
        "apiKey": "******",
        "secretKey": "******",
        "passphrase": "******",
        "perm": "read_only,trade",
        "ip": "1.1.1.1,2.2.2.2",
        "ts": "1597026383085"
    }]
}
```

### Query API Key of Sub-account
Applies to master accounts only.

**Rate Limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Read  

#### HTTP Request
`GET /api/v5/users/subaccount/apikey`

#### Request Example
```
GET /api/v5/users/subaccount/apikey?subAcct=panpanBroker2
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| apiKey | String | No | API public key |

#### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "label": "v5",
            "apiKey": "******",
            "perm": "read_only,trade",
            "ip": "1.1.1.1,2.2.2.2",
            "ts": "1597026383085"
        },
        {
            "label": "v5.1",
            "apiKey": "******",
            "perm": "read_only",
            "ip": "1.1.1.1,2.2.2.2",
            "ts": "1597026383085"
        }
    ]
}
```

## Reset the API Key of a sub-account

Applies to master accounts only and master accounts API Key must be linked to IP addresses.

**Rate limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

### HTTP Request
`POST /api/v5/users/subaccount/modify-apikey`

### Request Example
```
POST /api/v5/users/subaccount/modify-apikey
body
{
    "subAcct":"yongxu",
    "apiKey":"******",
    "ip":"1.1.1.1"
}
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| apiKey | String | Yes | Sub-account APIKey |
| label | String | No | Sub-account API Key label. The label will be reset if this is passed through. |
| perm | String | No | Sub-account API Key permissions (read_only: Read, trade: Trade). Separate with commas if more than one. The permission will be reset if this is passed through. |
| ip | String | No | Sub-account API Key linked IP addresses, separate with commas if more than one. Support up to 20 IP addresses. The IP will be reset if this is passed through. If ip is set to "", then no IP addresses is linked to the APIKey. |

### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "subAcct": "yongxu",
        "label": "v5",
        "apiKey": "******",
        "perm": "read,trade",
        "ip": "1.1.1.1",
        "ts": "1597026383085"
    }]
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| subAcct | String | Sub-account name |
| apiKey | String | Sub-account API public key |
| label | String | Sub-account API Key label |
| perm | String | Sub-account API Key permissions (read_only: Read, trade: Trade) |
| ip | String | Sub-account API Key IP addresses that linked with API Key |
| ts | String | Creation time |

## Delete the API Key of sub-accounts

Applies to master accounts only and master accounts API Key must be linked to IP addresses.

**Rate limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

### HTTP Request
`POST /api/v5/users/subaccount/delete-apikey`

### Request Example
```
POST /api/v5/users/subaccount/delete-apikey
body
{
    "subAcct":"test00001",
    "apiKey":"******"
}
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| apiKey | String | Yes | API public key |

### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "subAcct": "test00001"
    }]
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| subAcct | String | Sub-account name |

## Get sub-account trading balance

Query detailed balance info of Trading Account of a sub-account via the master account (applies to master accounts only).

**Rate limit**: 6 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`GET /api/v5/account/subaccount/balances`

### Request Example
```
GET /api/v5/account/subaccount/balances?subAcct=test1
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |

### Response Example
```
{
    "code": "0",
    "data": [
        {
            "adjEq": "101.46752000000001",
            "availEq": "624719833286",
            "borrowFroz": "0",
            "details": [
                {
                    "autoLendStatus": "off",
                    "autoLendMtAmt": "0",
                    "accAvgPx": "",
                    "availBal": "101.5",
                    "availEq": "101.5",
                    "borrowFroz": "0",
                    "cashBal": "101.5",
                    "ccy": "USDT",
                    "clSpotInUseAmt": "",
                    "crossLiab": "0",
                    "collateralEnabled": false,
                    "collateralRestrict": false,
                    "colBorrAutoConversion": "0",
                    "disEq": "101.46752000000001",
                    "eq": "101.5",
                    "eqUsd": "101.46752000000001",
                    "fixedBal": "0",
                    "frozenBal": "0",
                    "imr": "",
                    "interest": "0",
                    "isoEq": "0",
                    "isoLiab": "0",
                    "isoUpl": "0",
                    "liab": "0",
                    "maxLoan": "1015.0000000000001",
                    "maxSpotInUse": "",
                    "mgnRatio": "",
                    "mmr": "",
                    "notionalLever": "",
                    "openAvgPx": "",
                    "ordFrozen": "0",
                    "rewardBal": "",
                    "smtSyncEq": "0",
                    "spotBal": "",
                    "spotCopyTradingEq": "0",
                    "spotInUseAmt": "",
                    "spotIsoBal": "0",
                    "spotUpl": "",
                    "spotUplRatio": "",
                    "stgyEq": "0",
                    "totalPnl": "",
                    "totalPnlRatio": "",
                    "twap": "0",
                    "uTime": "1663854334734",
                    "upl": "0",
                    "uplLiab": "0"
                }
            ],
            "imr": "0",
            "isoEq": "0",
            "mgnRatio": "",
            "mmr": "0",
            "notionalUsd": "0",
            "notionalUsdForBorrow": "0",
            "notionalUsdForFutures": "0",
            "notionalUsdForOption": "0",
            "notionalUsdForSwap": "0",
            "ordFroz": "0",
            "totalEq": "101.46752000000001",
            "uTime": "1739332269934",
            "upl": "0"
        }
    ],
    "msg": ""
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| uTime | String | Update time of account information, millisecond format of Unix timestamp |
| totalEq | String | The total amount of equity in USD |
| isoEq | String | Isolated margin equity in USD |
| adjEq | String | Adjusted/Effective equity in USD |
| availEq | String | Account level available equity |
| ordFroz | String | Cross margin frozen for pending orders in USD |
| imr | String | Initial margin requirement in USD |
| mmr | String | Maintenance margin requirement in USD |
| borrowFroz | String | Potential borrowing IMR of the account in USD |
| mgnRatio | String | Maintenance margin ratio in USD |
| notionalUsd | String | Notional value of positions in USD |
| notionalUsdForBorrow | String | Notional value for Borrow in USD |
| notionalUsdForSwap | String | Notional value of positions for Perpetual Futures in USD |
| notionalUsdForFutures | String | Notional value of positions for Expiry Futures in USD |
| notionalUsdForOption | String | Notional value of positions for Option in USD |
| upl | String | Cross-margin info of unrealized profit and loss at the account level in USD |
| details | Array | Detailed asset information in all currencies |

## Get sub-account funding balance

Query detailed balance info of Funding Account of a sub-account via the master account (applies to master accounts only).

**Rate limit**: 6 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`GET /api/v5/asset/subaccount/balances`

### Request Example
```
GET /api/v5/asset/subaccount/balances?subAcct=test1
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| ccy | String | No | Single currency or multiple currencies (no more than 20) separated with comma |

### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
            "availBal": "37.11827078",
            "bal": "37.11827078",
            "ccy": "ETH",
            "frozenBal": "0"
        }
    ]
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency |
| bal | String | Balance |
| frozenBal | String | Frozen balance |
| availBal | String | Available balance |

## Get sub-account maximum withdrawals

Retrieve the maximum withdrawal information of a sub-account via the master account (applies to master accounts only). If no currency is specified, the transferable amount of all owned currencies will be returned.

**Rate limit**: 20 requests per 2 seconds  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`GET /api/v5/account/subaccount/max-withdrawal`

### Request Example
```
GET /api/v5/account/subaccount/max-withdrawal?subAcct=test1
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name |
| ccy | String | No | Single currency or multiple currencies (no more than 20) separated with comma |

### Response Example
```
{
   "code":"0",
   "data":[
      {
         "ccy":"BTC",
         "maxWd":"3",
         "maxWdEx":"",
         "spotOffsetMaxWd":"3",
         "spotOffsetMaxWdEx":""
      },
      {
         "ccy":"ETH",
         "maxWd":"15",
         "maxWdEx":"",
         "spotOffsetMaxWd":"15",
         "spotOffsetMaxWdEx":""
      },
      {
         "ccy":"USDT",
         "maxWd":"10600",
         "maxWdEx":"",
         "spotOffsetMaxWd":"10600",
         "spotOffsetMaxWdEx":""
      }
   ],
   "msg":""
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency |
| maxWd | String | Max withdrawal (excluding borrowed assets under Multi-currency margin) |
| maxWdEx | String | Max withdrawal (including borrowed assets under Multi-currency margin) |
| spotOffsetMaxWd | String | Max withdrawal under Spot-Derivatives risk offset mode (excluding borrowed assets under Portfolio margin) |
| spotOffsetMaxWdEx | String | Max withdrawal under Spot-Derivatives risk offset mode (including borrowed assets under Portfolio margin) |

## Get history of sub-account transfer

This endpoint is only available for master accounts. Transfer records are available from September 28, 2022 onwards.

**Rate limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

### HTTP Request
`GET /api/v5/asset/subaccount/bills`

### Request Example
```
GET /api/v5/asset/subaccount/bills
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | No | Currency |
| type | String | No | Transfer type (0: Master to sub-account, 1: Sub-account to master) |
| subAcct | String | No | Sub-account name |
| after | String | No | Query data earlier than timestamp (Unix ms) |
| before | String | No | Query data newer than timestamp (Unix ms) |
| limit | String | No | Number of results (max 100, default 100) |

### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
      {
        "amt": "1.1",
        "billId": "89887685",
        "ccy": "USDT", 
        "subAcct": "hahatest1",
        "ts": "1712560959000",
        "type": "0"
      }
    ]
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| billId | String | Bill ID |
| ccy | String | Transfer currency |
| amt | String | Transfer amount |
| type | String | Bill type |
| subAcct | String | Sub-account name |
| ts | String | Bill ID creation time (Unix ms) |

## Master accounts manage the transfers between sub-accounts

Applies to master accounts only. Only API keys with Trade privilege can call this endpoint.

**Rate limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

### HTTP Request
`POST /api/v5/asset/subaccount/transfer`

### Request Example
```
POST /api/v5/asset/subaccount/transfer
body
{
    "ccy":"USDT",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "fromSubAccount":"test-1",
    "toSubAccount":"test-2"
}
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | Yes | Currency |
| amt | String | Yes | Transfer amount |
| from | String | Yes | Account type of transfer from sub-account (6: Funding, 18: Trading) |
| to | String | Yes | Account type of transfer to sub-account (6: Funding, 18: Trading) |
| fromSubAccount | String | Yes | Sub-account name transferring out |
| toSubAccount | String | Yes | Sub-account name transferring in |
| loanTrans | Boolean | No | Whether borrowed coins can be transferred out (default false) |
| omitPosRisk | String | No | Ignore position risk (default false) |

### Response Example
```
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "transId":"12345"
        }
    ]
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| transId | String | Transfer ID |

## Set permission of transfer out

Set permission of transfer out for sub-account (only applicable to master account API key). Sub-account can transfer out to master account by default.

**Rate Limit**: 1 request per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

### HTTP Request
`POST /api/v5/users/subaccount/set-transfer-out`

### Request Example
```
POST /api/v5/users/subaccount/set-transfer-out
body
{
    "subAcct": "Test001,Test002",
    "canTransOut": true
}
```

### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| subAcct | String | Yes | Sub-account name(s), comma-separated (max 20) |
| canTransOut | Boolean | No | Transfer out permission (default true) |

### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "subAcct": "Test001",
            "canTransOut": true
        },
        {
            "subAcct": "Test002",
            "canTransOut": true
        }
    ]
}
```

### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| subAcct | String | Sub-account name |
| canTransOut | Boolean | Transfer out permission |

## Financial Product - On-chain earn

Only the assets in the funding account can be used for purchase.

### GET / Offers
**Rate Limit**: 3 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

#### HTTP Request
`GET /api/v5/finance/staking-defi/offers`

#### Request Example
```
GET /api/v5/finance/staking-defi/offers
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| productId | String | No | Product ID |
| protocolType | String | No | Protocol type (defi: on-chain earn) |
| ccy | String | No | Investment currency |

#### Response Example
```
{
    "code": "0",
    "data": [
        {
            "ccy": "DOT",
            "productId": "101",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1767",
            "earlyRedeem": false,
            "state": "purchasable",
            "investData": [
                {
                    "bal": "0",
                    "ccy": "DOT",
                    "maxAmt": "0",
                    "minAmt": "2"
                }
            ],
            "earningData": [
                {
                    "ccy": "DOT",
                    "earningType": "0"
                }
            ],
            "fastRedemptionDailyLimit": "",
            "redeemPeriod": [
                "28D",
                "28D"
            ]
        }
    ],
    "msg": ""
}
```

#### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ccy | String | Currency |
| productId | String | Product ID |
| protocol | String | Protocol |
| protocolType | String | Protocol type |
| term | String | Protocol term (0 for flexible) |
| apy | String | Estimated annualization |
| earlyRedeem | Boolean | Early redemption support |
| state | String | Product state |
| investData | Array | Investment data |
| earningData | Array | Earning data |
| fastRedemptionDailyLimit | String | Fast redemption daily limit |
| redeemPeriod | Array | Redemption period |

### POST / Purchase
**Rate Limit**: 2 requests per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

#### HTTP Request
`POST /api/v5/finance/staking-defi/purchase`

#### Request Example
```
POST /api/v5/finance/staking-defi/purchase
body 
{
    "productId":"1234",
    "investData":[
      {
        "ccy":"ZIL",
        "amt":"100"
      }
    ],
    "term":"30"
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| productId | String | Yes | Product ID |
| investData | Array | Yes | Investment data |
| term | String | Conditional | Investment term |
| tag | String | No | Order tag |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "ordId": "754147",
      "tag": ""
    }
  ]
}
```

#### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ordId | String | Order ID |
| tag | String | Order tag |

### POST / Redeem
**Rate Limit**: 2 requests per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

#### HTTP Request
`POST /api/v5/finance/staking-defi/redeem`

#### Request Example
```
POST /api/v5/finance/staking-defi/redeem
body 
{
    "ordId":"754147",
    "protocolType":"defi",
    "allowEarlyRedeem":true
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ordId | String | Yes | Order ID |
| protocolType | String | Yes | Protocol type |
| allowEarlyRedeem | Boolean | No | Early redemption flag |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "ordId": "754147",
      "tag": ""
    }
  ]
}
```

#### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ordId | String | Order ID |
| tag | String | Order tag |

### POST / Cancel purchases/redemptions
After cancelling, returning funds will go to the funding account.

**Rate Limit**: 2 requests per second  
**Rate limit rule**: User ID  
**Permission**: Trade  

#### HTTP Request
`POST /api/v5/finance/staking-defi/cancel`

#### Request Example
```
POST /api/v5/finance/staking-defi/cancel
body 
{
    "ordId":"754147",
    "protocolType":"defi"
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ordId | String | Yes | Order ID |
| protocolType | String | Yes | Protocol type |

#### Response Example
```
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "ordId": "754147",
      "tag": ""
    }
  ]
}
```

#### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ordId | String | Order ID |
| tag | String | Order tag |

### GET / Active orders
**Rate Limit**: 3 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

#### HTTP Request
`GET /api/v5/finance/staking-defi/orders-active`

#### Request Example
```
GET /api/v5/finance/staking-defi/orders-active
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| productId | String | No | Product ID |
| protocolType | String | No | Protocol type |
| ccy | String | No | Investment currency |
| state | String | No | Order state |

#### Response Example
```
{
    "code": "0",
    "data": [
        {
            "ordId": "2413499",
            "ccy": "DOT",
            "productId": "101",
            "state": "1",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1014",
            "investData": [
                {
                    "ccy": "DOT",
                    "amt": "2"
                }
            ],
            "earningData": [
                {
                    "ccy": "DOT",
                    "earningType": "0",
                    "earnings": "0.10615025"
                }
            ],
            "purchasedTime": "1729839328000",
            "tag": "",
            "estSettlementTime": "",
            "cancelRedemptionDeadline": "",
            "fastRedemptionData": []
        }
    ],
    "msg": ""
}
```

#### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ordId | String | Order ID |
| ccy | String | Currency |
| productId | String | Product ID |
| state | String | Order state |
| protocol | String | Protocol |
| protocolType | String | Protocol type |
| term | String | Protocol term |
| apy | String | Estimated APY |
| investData | Array | Investment data |
| earningData | Array | Earning data |
| purchasedTime | String | Order purchased time |
| tag | String | Order tag |

### GET / Order history
**Rate Limit**: 3 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

#### HTTP Request
`GET /api/v5/finance/staking-defi/orders-history`

#### Request Example
```
GET /api/v5/finance/staking-defi/orders-history
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| productId | String | No | Product ID |
| protocolType | String | No | Protocol type |
| ccy | String | No | Investment currency |
| after | String | No | Pagination (ordId) |
| before | String | No | Pagination (ordId) |
| limit | String | No | Number of results |

#### Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [
       {
            "ordId": "1579252",
            "ccy": "DOT",
            "productId": "101",
            "state": "3",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1704",
            "investData": [
                {
                    "ccy": "DOT",
                    "amt": "2"
                }
            ],
            "earningData": [
                {
                    "ccy": "DOT",
                    "earningType": "0",
                    "realizedEarnings": "0"
                }
            ],
            "purchasedTime": "1712908001000",
            "redeemedTime": "1712914294000",
            "tag": ""
       }
    ]
}
```

#### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| ordId | String | Order ID |
| ccy | String | Currency |
| productId | String | Product ID |
| state | String | Order state |
| protocol | String | Protocol |
| protocolType | String | Protocol type |
| term | String | Protocol term |
| apy | String | Estimated APY |
| investData | Array | Investment data |
| earningData | Array | Earning data |
| purchasedTime | String | Order purchased time |
| redeemedTime | String | Order redeemed time |
| tag | String | Order tag |

## Status

### GET / Status
Get event status of system upgrade.

**Rate Limit**: 1 request per 5 seconds  

#### HTTP Request
`GET /api/v5/system/status`

#### Request Example
```
GET /api/v5/system/status
```

#### Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| state | String | No | System maintenance status |

#### Response Example
```
{
    "code": "0",
    "data": [
        {
            "begin": "1672823400000",
            "end": "1672823520000",
            "href": "",
            "preOpenBegin": "",
            "scheDesc": "",
            "serviceType": "8",
            "state": "completed",
            "maintType": "1",
            "env": "1",
            "system": "unified",
            "title": "Trading account system upgrade (in batches of accounts)"
        }
    ],
    "msg": ""
}
```

#### Response Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| title | String | Maintenance title |
| state | String | Maintenance status |
| begin | String | Begin time |
| end | String | End time |
| preOpenBegin | String | Pre-open time |
| href | String | Details link |
| serviceType | String | Service type |
| system | String | System |
| scheDesc | String | Rescheduled description |
| maintType | String | Maintenance type |
| env | String | Environment |

### WS / Status channel
Get the status of system maintenance and receive push notifications when rescheduling or when system maintenance status and end time changes.

- **First subscription:** Push the latest change data.
- **Subsequent pushes:** Every time there is a state change, push the changed content.
- **Note:** Planned maintenance causing short interruptions (less than 5 seconds) or websocket disconnections (users can immediately reconnect) will not be announced.
- Maintenance only occurs during times of low market volatility.

---

## URL Path

`/ws/v5/public`

---

## Request Example

```
{
  "id": "unique-request-id",
  "op": "subscribe",
  "args": [
    {
      "channel": "status"
    }
  ]
}
```

---

## Request Parameters

| Parameter | Type   | Required | Description                                                                                     |
|-----------|--------|----------|-------------------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message. Provided by client. Returned in response for tracking.          |
| op        | String | Yes      | `subscribe` or `unsubscribe`                                                                  |
| args      | Array  | Yes      | List of subscribed channels                                                                     |
| > channel | String | Yes      | Channel name, here: `status`                                                                    |

---


## Response Parameters

| Parameter | Type   | Description                                  |
|-----------|--------|----------------------------------------------|
| id        | String | Unique identifier of the message             |
| event     | String | `subscribe` / `unsubscribe` / `error`       |
| arg       | Object | Subscribed channel                            |
| > channel | String | Channel name: `status`                        |
| code      | String | Error code (if any)                           |
| msg       | String | Error message (if any)                        |
| connId    | String | WebSocket connection ID                       |

---

## Push Data Example

### Push data parameters

| Parameter      | Type     | Description                                                                                              |
|----------------|----------|----------------------------------------------------------------------------------------------------------|
| arg            | Object   | Successfully subscribed channel                                                                          |
| > channel      | String   | Channel name                                                                                             |
| data           | Array    | Array of objects with subscribed data                                                                   |

### Subscribed data object fields

| Parameter      | Type   | Description                                                                                              |
|----------------|--------|----------------------------------------------------------------------------------------------------------|
| title          | String | Title of system maintenance instructions                                                                |
| state          | String | System maintenance status: `scheduled` (waiting) `ongoing` (processing) `pre_open` (pre-open) `completed` (finished) `canceled` (canceled) Note: `pre_open` generally lasts about 10 minutes and occurs when upgrade duration is long. |
| begin          | String | Start time of system maintenance, Unix timestamp in milliseconds, e.g. `1617788463867`                   |
| end            | String | Resume trading time. Expected end time before `completed`; actual end time after `completed`. Unix timestamp in milliseconds. |
| preOpenBegin   | String | Time of `pre_open` phase. Client actions such as canceling orders, placing Post Only orders, and transferring funds to trading accounts resume at this point. |
| href           | String | Hyperlink for system maintenance details. Empty if none.                                                |
| serviceType    | String | Service type:`0`: WebSocket `5`: Trading service `6`: Block trading `7`: Trading bot `8`: Trading service (batch accounts) `9`: Trading service (batch products) `10`: Spread trading `11`: Copy trading `99`: Others (e.g. suspend partial instruments) |
| system         | String | System affected, e.g. `unified` (Trading account)                                                       |
| scheDesc       | String | Rescheduled description, e.g. `Rescheduled from 2021-01-26T16:30:00.000Z to 2021-01-28T16:30:00.000Z`    |
| maintType      | String | Maintenance type: `1`: Scheduled maintenance `2`: Unscheduled maintenance `3`: System disruption |
| env            | String | Environment: `1`: Production Trading `2`: Demo Trading                                         |
| ts             | String | Push time due to change event, Unix timestamp in milliseconds, e.g. `1617788463867`                     |

---

# Announcement API

## Get announcements

- Response sorted by `pTime` with the most recent first.
- Sorting is not affected by updates to announcements.
- 20 records per page.
- Authentication optional. Responses filtered based on request IP if public, or country if private.
- Rate limit: 5 requests per 2 seconds (User ID for private, IP for public).
- Permission: Read

### HTTP Request

`GET /api/v5/support/announcements`

### Request Parameters

| Parameter | Type   | Required | Description                           |
|-----------|--------|----------|---------------------------------------|
| annType   | String | No       | Announcement type. See "Announcement types" endpoint. Returns all if not provided. |
| page      | String | No       | Page number for pagination. Default: 1 |

### Response Parameters

| Parameter  | Type   | Description                                  |
|------------|--------|----------------------------------------------|
| totalPage  | String | Total number of pages                         |
| details    | Array  | List of announcements                         |
| > title    | String | Announcement title                            |
| > annType  | String | Announcement type                             |
| > pTime    | String | Publish time, Unix timestamp in milliseconds |
| > url      | String | Announcement URL                              |

---

## Get announcement types

- Authentication not required.
- Rate limit: 1 request per 2 seconds (IP).
- Permission: Read

### HTTP Request

`GET /api/v5/support/announcement-types`

### Request Parameters

None

### Response Parameters

| Parameter    | Type   | Description               |
|--------------|--------|---------------------------|
| annType      | String | Announcement type         |
| annTypeDesc  | String | Description of the type   |

Error Code
Here is the REST API Error Code

REST API
REST API Error Code is from 50000 to 59999.

Public
Error Code from 50000 to 53999

General Class
Error Code	HTTP Status Code	Error Message
0	200	
1	200	Operation failed.
2	200	Bulk operation partially succeeded.
50000	400	Body for POST request cannot be empty.
50001	503	Service temporarily unavailable. Please try again later.
50002	400	JSON syntax error
50004	400	API endpoint request timeout. (does not mean that the request was successful or failed, please check the request result).
50005	410	API endpoint is inactive or unavailable.
50006	400	Invalid Content-Type. Please use "application/JSON".
50007	200	User blocked.
50008	200	User doesn't exist.
50009	200	Account is frozen due to stop-out.
50010	200	User ID cannot be empty.
50011	200	Rate limit reached. Please refer to API documentation and throttle requests accordingly.
50011	429	Too Many Requests
50012	200	Account status invalid. Check account status
50013	429	Systems are busy. Please try again later.
50014	400	Parameter {param0} can not be empty.
50015	400	Either parameter {param0} or {param1} is required
50016	400	Parameter {param0} does not match parameter {param1}
50017	200	Position frozen and related operations restricted due to auto-deleveraging (ADL). Please try again later.
50018	200	Currency {param0} is frozen due to ADL. Operation restricted.
50019	200	Account frozen and related operations restricted due to auto-deleveraging (ADL). Please try again later.
50020	200	Position frozen and related operations restricted due to forced liquidation. Please try again later.
50021	200	Currency {param0} is frozen due to liquidation. Operation restricted.
50022	200	Account frozen and related operations restricted due to forced liquidation. Please try again later.
50023	200	Funding fees frozen and related operations are restricted. Please try again later.
50024	200	Parameter {param0} and {param1} can not exist at the same time.
50025	200	Parameter {param0} count exceeds the limit {param1}.
50026	500	System error. Try again later
50027	200	This account is restricted from trading. Please contact customer support for assistance.
50028	200	Unable to place the order. Please contact the customer service for details.
50029	200	Your account has triggered OKX risk control and is temporarily restricted from conducting transactions. Please check your email registered with OKX for contact from our customer support team.
50030	200	You don't have permission to use this API endpoint
50032	200	Your account has been set to prohibit transactions in this currency. Please confirm and try again
50033	200	Instrument blocked. Please verify trading this instrument is allowed under account settings and try again.
50035	403	This endpoint requires that APIKey must be bound to IP
50036	200	The expTime can't be earlier than the current system time. Please adjust the expTime and try again.
50037	200	Order expired.
50038	200	This feature is unavailable in demo trading
50039	200	Parameter "before" isn't supported for timestamp pagination
50040	200	Too frequent operations, please try again later
50041	200	Your user ID hasn’t been allowlisted. Please contact customer service for assistance.
50042	200	Repeated request
50044	200	Must select one broker type
50045	200	simPos should be empty because simulated positions cannot be counted when Position Builder is calculating under Spot-Derivatives risk offset mode
50046	200	This feature is temporarily unavailable while we make some improvements to it. Please try again later.
50047	200	{param0} has already settled. To check the relevant candlestick data, please use {param1}
50048	200	Switching risk unit may lead position risk increases and be forced liquidated. Please adjust position size, make sure margin is in a safe status.
50049	200	No information on the position tier. The current instrument doesn’t support margin trading.
50050	200	You’ve already activated options trading. Please don’t activate it again.
50051	200	Due to compliance restrictions in your country or region, you cannot use this feature.
50052	200	Due to local laws and regulations, you cannot trade with your chosen crypto.
50053	200	This feature is only available in demo trading.
50055	200	Reset unsuccessful. Assets can only be reset up to 5 times per day.
50056	200	You have pending orders or open positions with this currency. Please reset after canceling all the pending orders/closing all the open positions.
50057	200	Reset unsuccessful. Try again later.
50058	200	This crypto is not supported in an asset reset.
50059	200	Before you continue, you'll need to complete additional steps as required by your local regulators. Please visit the website or app for more details.
50060	200	For security and compliance purposes, please complete the identity verification process to continue using our services.
50061	200	You've reached the maximum order rate limit for this account.
50062	200	This feature is currently unavailable.
50063	200	You can't activate the credits as they might have expired or are already activated.
50064	200	The borrowing system is unavailable. Try again later.
50067	200	The API doesn't support cross site trading feature
50069	200	Margin ratio verification for this risk unit failed.
50071	200	{param} already exists
e.g. clOrdId already exists
API Class
Error Code	HTTP Status Code	Error Message
50100	400	API frozen, please contact customer service.
50101	401	APIKey does not match current environment.
50102	401	Timestamp request expired.
50103	401	Request header "OK-ACCESS-KEY" cannot be empty.
50104	401	Request header "OK-ACCESS-PASSPHRASE" cannot be empty.
50105	401	Request header "OK-ACCESS-PASSPHRASE" incorrect.
50106	401	Request header "OK-ACCESS-SIGN" cannot be empty.
50107	401	Request header "OK-ACCESS-TIMESTAMP" cannot be empty.
50108	401	Exchange ID does not exist.
50109	401	Exchange domain does not exist.
50110	401	Your IP {param0} is not included in your API key's IP whitelist.
50111	401	Invalid OK-ACCESS-KEY.
50112	401	Invalid OK-ACCESS-TIMESTAMP.
50113	401	Invalid signature.
50114	401	Invalid authorization.
50115	405	Invalid request method.
50116	200	Fast API is allowed to create only one API key
50118	200	To link the app using your API key, your broker needs to share their IP to be whitelisted
50119	200	API key doesn't exist
50120	200	This API key doesn't have permission to use this function
50121	200	You can't access our services through the IP address ({param0})
50122	200	Order amount must exceed minimum amount
Trade Class
Error Code	HTTP Status code	Error Message
51000	400	Parameter {param0} error
51001	200	Instrument ID or Spread ID doesn't exist.
51002	200	Instrument ID doesn't match underlying index.
51003	200	Either client order ID or order ID is required.
51004	200	Order failed. For isolated long/short mode of {param0}, the sum of current order size, position quantity in the same direction, and pending orders in the same direction can’t be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, current order size: {param3} contracts, position quantity in the same direction: {param4} contracts, pending orders in the same direction: {param5} contracts).
51004	200	Order failed. For cross long/short mode of {param0}, the sum of current order size, position quantity in the long and short directions, and pending orders in the long and short directions can’t be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, current order size: {param3} contracts, position quantity in the long and short directions: {param4} contracts, pending orders in the long and short directions: {param5} contracts).
51004	200	Order failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of current order size, current instId position quantity in the long and short directions, current instId pending orders in the long and short directions, and other contracts of the same instFamily can’t be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, current order size: {param4} contracts, current instId position quantity in the long and short directions: {param5} contracts, current instId pending orders in the long and short directions: {param6} contracts, other contracts of the same instFamily: {param7} contracts).
51004	200	Order failed. For buy/sell mode of {param0}, the sum of current buy order size, position quantity, and pending buy orders can’t be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, current buy order size: {param3} contracts, position quantity: {param4} contracts, pending buy orders: {param5} contracts).
51004	200	Order failed. For buy/sell mode of {param0}, the sum of current sell order size, position quantity, and pending sell orders can’t be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, current sell order size: {param3} contracts, position quantity: {param4} contracts, pending sell orders: {param5} contracts).
51004	200	Order failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of current buy order size, current instId position quantity, current instId pending buy orders, and other contracts of the same instFamily can’t be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, current buy order size: {param4} contracts, current instId position quantity: {param5} contracts, current instId pending buy orders: {param6} contracts, other contracts of the same instFamily: {param7} contracts).
51004	200	Order failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of current sell order size, current instId position quantity, current instId pending sell orders, and other contracts of the same instFamily can’t be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, current sell order size: {param4} contracts, current instId position quantity: {param5} contracts, current instId pending sell orders: {param6} contracts, other contracts of the same instFamily: {param7} contracts).
51004	200	Order amendment failed. For isolated long/short mode of {param0}, the sum of increment order size by amendment, position quantity in the same direction, and pending orders in the same direction can’t be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, increment order size by amendment: {param3} contracts, position quantity in the same direction: {param4} contracts, pending orders in the same direction: {param5} contracts).
51004	200	Order amendment failed. For cross long/short mode of {param0}, the sum of increment order size by amendment, position quantity in the long and short directions, and pending orders in the long and short directions can’t be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, increment order size by amendment: {param3} contracts, position quantity in the long and short directions: {param4} contracts, pending orders in the same direction: {param5} contracts).
51004	200	Order amendment failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of increment order size by amendment, current instId position quantity in the long and short directions, current instId pending orders in the long and short directions, and other contracts of the same instFamily can’t be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, increment order size by amendment: {param4} contracts, current instId position quantity in the long and short directions: {param5} contracts, current instId pending orders in the long and short directions: {param6} contracts, other contracts of the same instFamily: {param7} contracts).
51004	200	Order amendment failed. For buy/sell mode of {param0}, the sum of increment order size by amending current buy order, position quantity, and pending buy orders can’t be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, increment order size by amending current buy order: {param3} contracts, position quantity: {param4} contracts, pending buy orders: {param5} contracts).
51004	200	Order amendment failed. For buy/sell mode of {param0}, the sum of increment order size by amending current sell order, position quantity, and pending sell orders can’t be more than {param1}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param2}×, increment order size by amending current sell order: {param3} contracts, position quantity: {param4} contracts, pending sell orders: {param5} contracts).
51004	200	Order amendment failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of increment order size by amending current buy order, current instId position quantity, current instId pending buy orders, and other contracts of the same instFamily can’t be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, increment order size by amending current buy order: {param4} contracts, current instId position quantity: {param5} contracts, current instId pending buy orders: {param6} contracts, other contracts of the same instFamily: {param7} contracts).
51004	200	Order amendment failed. For cross buy/sell mode of {param0} and instFamily {param1}, the sum of increment order size by amending current sell order, current instId position quantity, current instId pending sell orders, and other contracts of the same instFamily can’t be more than {param2}(contracts) which is the maximum position amount under current leverage. Please lower the leverage or use a new sub-account to place the order again (current leverage: {param3}×, increment order size by amending current sell order: {param4} contracts, current instId position quantity: {param5} contracts, current instId pending sell orders: {param6} contracts, other contracts of the same instFamily: {param7} contracts).
51005	200	Your order amount exceeds the max order amount.
51006	200	Order price is not within the price limit (max buy price: {param0} , min sell price: {param1} )
51007	200	Order failed. Please place orders of at least 1 contract or more.
51008	200	Order failed. Insufficient {param0} balance in account
51008	200	Order failed. Insufficient {param0} margin in account
51008	200	Order failed. Insufficient {param0} balance in account, and Auto Borrow is not enabled
51008	200	Order failed. Insufficient {param0} margin in account and auto-borrow is not enabled (Portfolio margin mode can try IOC orders to lower the risks)
51008	200	Insufficient {param0} available as your borrowing amount exceeds tier limit. Lower leverage appropriately. New and pending limit orders need borrowings {param1}, remaining quota {param2}, total limit {param3}, in use {param4}.
51008	200	Order failed. Exceeds {param0} borrow limit (Limit of master account plus the allocated VIP quota for the current account) (Existing pending orders and the new order are required to borrow {param1}, Remaining limit {param2}, Limit {param3}, Limit used {param4})
51008	200	Order failed. Insufficient {param0} borrowing quota results in an insufficient amount available to borrow.
51008	200	Order failed. Insufficient {param0} available in loan pool to borrow.
51008	200	Order failed. Insufficient account balance, and the adjusted equity in USD is less than IMR (Portfolio margin mode can try IOC orders to lower the risks)
51008	200	Order failed. The order didn't pass delta verification because if the order were to succeed, the change in adjEq would be smaller than the change in IMR. Increase adjEq or reduce IMR (Portfolio margin mode can try IOC orders to lower the risks)
51009	200	Order blocked. Please contact customer support for assistance.
51010	200	Request unsupported under current account mode
51011	200	Order ID already exists.
51012	200	Token doesn't exist.
51014	200	Index doesn't exist.
51015	200	Instrument ID doesn't match instrument type.
51016	200	Client order ID already exists.
51017	200	Loan amount exceeds borrowing limit.
51018	200	Users with options accounts cannot hold net short positions.
51019	200	No net long positions can be held under cross margin mode in options.
51020	200	Your order should meet or exceed the minimum order amount.
51021	200	The pair or contract is not yet listed
51022	200	Contract suspended.
51023	200	Position doesn't exist.
51024	200	Trading account is blocked.
51024	200	In accordance with the terms of service, we regret to inform you that we cannot provide services for you. If you have any questions, please contact our customer support.
51024	200	According to your request, this account has been frozen. If you have any questions, please contact our customer support.
51024	200	Your account has recently changed some security settings. To protect the security of your funds, this action is not allowed for now. If you have any questions, please contact our customer support.
51024	200	You have withdrawn all assets in the account. To protect your personal information, the account has been permanently frozen. If you have any questions, please contact our customer support.
51024	200	Your identity could not be verified. To protect the security of your funds, this action is not allowed. Please contact our customer support.
51024	200	Your verified age doesn't meet the requirement. To protect the security of your funds, we cannot proceed with your request. Please contact our customer support.
51024	200	In accordance with the terms of service, trading is currently unavailable in your verified country or region. Close all open positions or contact customer support if you have any questions.
51024	200	In accordance with the terms of service, multiple account is not allowed. To protect the security of your funds, this action is not allowed. Please contact our customer support.
51024	200	Your account is in judicial freezing, and this action is not allowed for now. If you have any questions, please contact our customer support.
51024	200	Based on your previous requests, this action is not allowed for now. If you have any questions, please contact our customer support.
51024	200	Your account has disputed deposit orders. To protect the security of your funds, this action is not allowed for now. Please contact our customer support.
51024	200	Unable to proceed. Please resolve your existing P2P disputes first.
51024	200	Your account might have compliance risk. To protect the security of your funds, this action is not allowed for now. Please contact our customer support.
51024	200	Based on your trading requests, this action is not allowed for now. If you have any questions, please contact our customer support.
51024	200	Your account has triggered risk control. This action is not allowed for now. Please contact our customer support.
51024	200	This account is temporarily unavailable. Please contact our customer support.
51024	200	Withdrawal function of this account is temporarily unavailable. Please contact our customer support.
51024	200	Transfer function of this account is temporarily unavailable. Please contact our customer support.
51024	200	You violated the "Fiat Trading Rules" when you were doing fiat trade, so we'll no longer provide fiat trading-related services for you. The deposit and withdrawal of your account and other trading functions will not be affected.
51024	200	Please kindly check your mailbox and reply to emails from the verification team.
51024	200	According to your request, this account has been closed. If you have any questions, please contact our customer support.
51024	200	Your account might have security risk. To protect the security of your funds, this action is not allowed for now. Please contact our customer support.
51024	200	Your account might have security risk. Convert is now unavailable. Please contact our customer support.
51024	200	Unable to proceed due to account restrictions. We've sent an email to your OKX registered email address regarding this matter, or you can contact customer support via Chat with AI chatbot on our support center page.
51024	200	In accordance with the terms of service, trading is currently unavailable in your verified country or region. Cancel all orders or contact customer support if you have any questions.
51024	200	In accordance with the terms of service, trading is not available in your verified country. If you have any questions, please contact our customer support.
51024	200	This product isn’t available in your country or region due to local laws and regulations. If you don’t reside in this area, you may continue using OKX Exchange products with a valid government-issued ID.
51024	200	Please note that you may not be able to transfer or trade in the first 30 minutes after establishing custody trading sub-accounts. Please kindly wait and try again later.
51024	200	Feature unavailable. Complete Advanced verification to access this feature.
51024	200	You can't trade or deposit now. Update your personal info to restore full account access immediately.
51024	200	Sub-accounts exceeding the limit aren't allowed to open new positions and can only reduce or close existing ones. Please try again with a different account.
51025	200	Order count exceeds the limit.
51026	200	Instrument type doesn't match underlying index.
51027	200	Contract expired.
51028	200	Contract under delivery.
51029	200	Contract is being settled.
51030	200	Funding fee is being settled.
51031	200	This order price is not within the closing price range.
51032	200	Closing all the positions at the market price.
51033	200	The total amount per order for this pair has reached the upper limit.
51034	200	Fill rate exceeds the limit that you've set. Please reset the market maker protection to inactive for new trades.
51035	200	This account doesn't have permission to submit MM quote order.
51036	200	Only options instrument of the PM account supports MMP orders.
51042	200	Under the Portfolio margin account, users can only place market maker protection orders in cross margin mode in Options.
51043	200	This isolated position doesn't exist.
59509	200	Account does not have permission to reset MMP status
51037	200	This account only supports placing IOC orders to reduce account risk.
51038	200	IOC order already exists under the current risk module.
51039	200	Leverage cannot be adjusted for the cross positions of Expiry Futures and Perpetual Futures under the PM account.
51040	200	Cannot adjust margins for long isolated options positions
51041	200	Portfolio margin account only supports the Buy/Sell mode.
51044	200	The order type {param0}, {param1} is not allowed to set stop loss and take profit
51046	200	The take profit trigger price must be higher than the order price
51047	200	The stop loss trigger price must be lower than the order price
51048	200	The take profit trigger price must be lower than the order price
51049	200	The stop loss trigger price must be higher than the order price
51050	200	The take profit trigger price must be higher than the best ask price
51051	200	The stop loss trigger price must be lower than the best ask price
51052	200	The take profit trigger price must be lower than the best bid price
51053	200	The stop loss trigger price must be higher than the best bid price
51054	500	Request timed out. Please try again.
51055	200	Futures Grid is not available in Portfolio Margin mode
51056	200	Action not allowed
51057	200	This bot isn’t available in current account mode. Switch mode in Settings > Account mode to continue.
51058	200	No available position for this algo order
51059	200	Strategy for the current state does not support this operation
51063	200	OrdId does not exist
51065	200	algoClOrdId already exists.
51066	200	Market orders unavailable for options trading. Place a limit order to close position.
51068	200	{param0} already exists within algoClOrdId and attachAlgoClOrdId.
51069	200	The option contracts related to current {param0} do not exist
51070	200	You do not meet the requirements for switching to this account mode. Please upgrade the account mode on the OKX website or App
51071	200	You've reached the maximum limit for tag level cancel all after timers.
51072	200	As a spot lead trader, you need to set tdMode to spot_isolated when buying the configured lead trade pairs.
51073	200	As a spot lead trader, you need to use '/copytrading/close-subposition' for selling assets through lead trades
51074	200	Only the tdMode for lead trade pairs configured by spot lead traders can be set to 'spot_isolated'
51075	200	Order modification failed. You can only modify the price of sell orders in spot copy trading.
51076	200	TP/SL orders in Split TPs only support one-way TP/SL. You can't use slTriggerPx&slOrdPx and tpTriggerPx&tpOrdPx at the same time.
51077	200	Setting multiple TP and cost-price SL orders isn’t supported for spot and margin trading.
51078	200	You are a lead trader. Split TPs are not supported.
51079	200	The number of TP orders with Split TPs attached in a same order cannot exceed {param0}
51080	200	Take-profit trigger price types (tpTriggerPxType) must be the same in an order with Split TPs attached
51081	200	Take-profit trigger prices (tpTriggerPx) cannot be the same in an order with Split TPs attached
51082	200	TP trigger prices (tpOrdPx) in one order with multiple TPs must be market prices.
51083	200	The total size of TP orders with Split TPs attached in a same order should equal the size of this order
51084	200	The number of SL orders with Split TPs attached in a same order cannot exceed {param0}
51085	200	The number of TP orders cannot be less than 2 when cost-price SL is enabled (amendPxOnTriggerType set as 1) for Split TPs
51086	200	The number of orders with Split TPs attached in a same order cannot exceed {param0}
51538	200	You need to use attachAlgoOrds if you used attachAlgoOrds when placing an order. attachAlgoOrds is not supported if you did not use attachAlgoOrds when placing this order.
51539	200	attachAlgoId or attachAlgoClOrdId cannot be identical when modifying any TP/SL within your split TPs order
51527	200	Order modification failed. At least 1 of the attached TP/SL orders does not exist.
51087	200	Listing canceled for this crypto
51088	200	You can only place 1 TP/SL order to close an entire position
51089	200	The size of the TP order among split TPs attached cannot be empty
51090	200	You can't modify the amount of an SL order placed with a TP limit order.
51091	200	All TP orders in one order must be of the same type.
51092	200	TP order prices (tpOrdPx) in one order must be different.
51093	200	TP limit order prices (tpOrdPx) in one order can't be –1 (market price).
51094	200	You can't place TP limit orders in spot, margin, or options trading.
51095	200	To place TP limit orders at this endpoint, you must place an SL order at the same time.
51096	200	cxlOnClosePos needs to be true to place a TP limit order
51098	200	You can't add a new TP order to an SL order placed with a TP limit order.
51099	200	You can't place TP limit orders as a lead trader.
51178	200	tpTriggerPx&tpOrdPx or slTriggerPx&slOrdPx can't be empty when using attachAlgoClOrdId.
51100	200	Unable to place order. Take profit/Stop loss conditions cannot be added to reduce-only orders.
51101	200	Order failed, the sz of the current order can’t be more than {param0} (contracts).
51102	200	Order failed, the number of pending orders for this instId can’t be more than {param0} (orders)
51103	200	Order failed, the number of pending orders across all instIds under the {param0} current instFamily can’t be more than {param1} (orders)
51104	200	Order failed, the aggregated contract quantity for all pending orders across all instIds under the {param0} current instFamily can’t be more than {param1} (contracts)
51105	200	Order failed, the maximal sum of position quantity and pending orders quantity with the same direction for current instId can’t be more than {param0} (contracts)
51106	200	Order failed, the maximal sum of position quantity and pending orders quantity with the same direction across all instIds under the {param0} current instFamily can’t be more than {param1} (contracts)
51107	200	Order failed, the maximal sum of position quantity and pending orders quantity in both directions across all instIds under the {param0} current instFamily can’t be more than {param1} (contracts)
51108	200	Positions exceed the limit for closing out with the market price.
51109	200	No available offers.
51110	200	You can only place a limit order after Call Auction has started.
51111	200	Maximum {param0} orders can be placed in bulk.
51112	200	Close order size exceeds available size for this position.
51113	429	Market-price liquidation requests too frequent.
51115	429	Cancel all pending close-orders before liquidation.
51116	200	Order price or trigger price exceeds {param0}.
51117	200	Pending close-orders count exceeds limit.
51120	200	Order amount is less than {param0}, please try again.
51121	200	Order quantity must be a multiple of the lot size.
51122	200	Order price should higher than the min price {param0}
51123	200	Min price increment is null.
51124	200	You can only place limit orders during call auction.
51125	200	Currently there are pending reduce + reverse position orders in margin trading. Please cancel all pending reduce + reverse position orders and continue.
51126	200	Currently there are pending reduce only orders in margin trading. Please cancel all pending reduce only orders and continue.
51127	200	Available balance is 0.
51128	200	Multi-currency margin accounts cannot do cross-margin trading.
51129	200	The value of the position and buy order has reached the position limit. No further buying is allowed.
51130	200	Fixed margin currency error.
51131	200	Insufficient balance.
51132	200	Your position amount is negative and less than the minimum trading amount.
51133	200	Reduce-only feature is unavailable for spot transactions in multi-currency margin accounts.
51134	200	Closing failed. Please check your margin holdings and pending orders. Turn off the Reduce-only to continue.
51135	200	Your closing price has triggered the limit price, and the max buy price is {param0}.
51136	200	Your closing price has triggered the limit price, and the min sell price is {param0}.
51137	200	The highest price limit for buy orders is {param0}.
51138	200	The lowest price limit for sell orders is {param0}.
51139	200	Reduce-only feature is unavailable for the spot transactions by spot mode.
51140	200	Purchase failed due to insufficient sell orders, please try later.
51142	200	There is no valid quotation in the market, and the order cannot be filled in USDT mode, please try to switch to currency mode
51143	200	Insufficient conversion amount
51144	200	Please use {param0} for closing.
51147	200	To trade options, make sure you have more than 10,000 USD worth of assets in your trading account first, then activate options trading
51148	200	Failed to place order. The new order may execute an opposite trading direction of your existing reduce-only positions. Cancel or edit pending orders to continue order
51149	500	Order timed out. Please try again.
51150	200	The precision of the number of trades or the price exceeds the limit.
51152	200	Unable to place an order that mixes automatic buy with automatic repayment or manual operation in Quick margin mode.
51153	200	Unable to borrow manually in Quick margin mode. The amount you entered exceeds the upper limit.
51154	200	Unable to repay manually in Quick margin mode. The amount you entered exceeds your available balance.
51155	200	Trading of this pair or contract is restricted due to local compliance requirements.
51158	200	Manual transfer unavailable. To proceed, please switch to Quick margin mode (isoMode = quick_margin)
51164	200	As lead trader, you can't switch to portfolio margin mode.
51169	200	Order failed because you don't have any positions in this direction for this contract to reduce or close.
51170	200	Failed to place order. A reduce-only order can’t be the same trading direction as your existing positions.
51171	200	Failed to edit order. The edited order may execute an opposite trading direction of your existing reduce-only positions. Cancel or edit pending orders to continue.
51173	200	Unable to close all at market price. Your current positions don't have any liabilities.
51174	200	Order failed, number of pending orders for {param0} exceed the limit of {param1}.
51175	200	Parameters {param0} {param1} and {param2} cannot be empty at the same time
51176	200	Only one parameter can be filled among Parameters {param0} {param1} and {param2}
51177	200	Unavailable to amend {param1} because the price type of the current options order is {param0}
51179	200	Unavailable to place options orders using {param0} in simple mode
51180	200	The range of {param0} should be ({param1}~{param2})
51181	200	ordType must be limit while placing {param0} orders
51182	200	The total number of pending orders under price types pxUsd and pxVol for the current account cannot exceed {param0}
51185	200	The maximum value allowed per order is {maxOrderValue} USD
51186	200	Order failed. The leverage for {param0} in your current margin mode is {param1}x, which exceeds the platform limit of {param2}x.
51187	200	Order failed. For {param0} {param1} in your current margin mode, the sum of your current order amount, position sizes, and open orders is {param2} contracts, which exceeds the platform limit of {param3} contracts. Reduce your order amount, cancel orders, or close positions.
51192	200	The {param0} price corresponding to the IV level you entered is lower than the minimum allowed selling price of {param1} {param2}. Enter a higher IV level.
51193	200	The {param0} price corresponding to the IV level you entered is higher than the maximum allowed buying price of {param1} {param2}. Enter a lower IV level.
51194	200	The {param0} price corresponding to the USD price you entered is lower than the minimum allowed selling price of {param1} {param2}. Enter a higher USD price.
51195	200	The {param0} price corresponding to the USD price you entered is higher than the maximum allowed buying price of {param1} {param2}. Enter a lower USD price.
51196	200	You can only place limit orders during the pre-quote phase.
51197	200	You can only place limit orders after the pre-quote phase begins.
51201	200	The value of a market order can't exceed {param0}.
51202	200	Market order amount exceeds the maximum amount.
51203	200	Order amount exceeds the limit {param0}.
51204	200	The price for the limit order cannot be empty.
51205	200	Reduce Only is not available.
51206	200	Please cancel the Reduce Only order before placing the current {param0} order to avoid opening a reverse position.
51207	200	Trading amount exceeds the limit, and can't be all closed at the market price. You can try closing the position manually in batches.
51220	200	Lead and follow bots only support “Sell” or “Close all positions” when bot stops
51221	200	The profit-sharing ratio must be between 0% and 30%
51222	200	Profit sharing isn’t supported for this type of bot
51223	200	Only lead bot creators can set profit-sharing ratio
51224	200	Profit sharing isn’t supported for this crypto pair
51225	200	Instant trigger isn’t available for follow bots
51226	200	Editing parameters isn’t available for follow bots
51250	200	Algo order price is out of the available range.
51251	200	Bot order type error occurred when placing iceberg order
51252	200	Algo order amount is out of the available range.
51253	200	Average amount exceeds the limit of per iceberg order.
51254	200	Iceberg average amount error occurred.
51255	200	Limit of per iceberg order: Total amount/1000 < x <= Total amount.
51256	200	Iceberg order price variance error.
51257	200	Trailing stop order callback rate error. The callback rate should be {min}< x<={max}%.
51258	200	Trailing stop order placement failed. The trigger price of a sell order must be higher than the last transaction price.
51259	200	Trailing stop order placement failed. The trigger price of a buy order must be lower than the last transaction price.
51260	200	Maximum of {param0} pending trailing stop orders can be held at the same time.
51261	200	Each user can hold up to {param0} pending stop orders at the same time.
51262	200	Maximum {param0} pending iceberg orders can be held at the same time.
51263	200	Maximum {param0} pending time-weighted orders can be held at the same time.
51264	200	Average amount exceeds the limit of per time-weighted order.
51265	200	Time-weighted order limit error.
51267	200	Time-weighted order strategy initiative rate error.
51268	200	Time-weighted order strategy initiative range error.
51269	200	Time-weighted order interval error. Interval must be {%min}<= x<={%max}.
51270	200	The limit of time-weighted order price variance is 0 < x <= 1%.
51271	200	Sweep ratio must be 0 < x <= 100%.
51272	200	Price variance must be 0 < x <= 1%.
51273	200	Total amount must be greater than {param0}.
51274	200	Total quantity of time-weighted order must be larger than single order limit.
51275	200	The amount of single stop-market order cannot exceed the upper limit.
51276	200	Prices cannot be specified for stop market orders.
51277	200	TP trigger price cannot be higher than the last price.
51278	200	SL trigger price cannot be lower than the last price.
51279	200	TP trigger price cannot be lower than the last price.
51280	200	SL trigger price cannot be higher than the last price.
51281	200	Trigger order do not support the tgtCcy parameter.
51282	200	The range of Price variance is {param0}~{param1}
51283	200	The range of Time interval is {param0}~{param1}
51284	200	The range of Average amount is {param0}~{param1}
51285	200	The range of Total amount is {param0}~{param1}
51286	200	The total amount should not be less than {param0}
51287	200	This bot doesn't support current instrument
51288	200	Bot is currently stopping. Do not make multiple attempts to stop.
51289	200	Bot configuration does not exist. Please try again later
51290	200	The Bot engine is being upgraded. Please try again later
51291	200	This Bot does not exist or has been stopped
51292	200	This Bot type does not exist
51293	200	This Bot does not exist
51294	200	This Bot cannot be created temporarily. Please try again later
51295	200	Portfolio margin account does not support ordType {param0} in Trading bot mode
51298	200	Trigger orders are not available in the net mode of Expiry Futures and Perpetual Futures
51299	200	Order did not go through. You can hold a maximum of {param0} orders of this type.
51300	200	TP trigger price cannot be higher than the mark price
51302	200	SL trigger price cannot be lower than the mark price
51303	200	TP trigger price cannot be lower than the mark price
51304	200	SL trigger price cannot be higher than the mark price
51305	200	TP trigger price cannot be higher than the index price
51306	200	SL trigger price cannot be lower than the index price
51307	200	TP trigger price cannot be lower than the index price
51308	200	SL trigger price cannot be higher than the index price
51309	200	Cannot create trading bot during call auction
51310	200	Strategic orders with Iceberg and TWAP order type are not supported when margins are self-transferred in isolated mode.
51311	200	Failed to place trailing stop order. Callback rate should be within {min}<x<={max}
51312	200	Failed to place trailing stop order. Order amount should be within {min}<x<={max}
51313	200	Manual transfer in isolated mode does not support bot trading
51317	200	Trigger orders are not available by margin
51327	200	closeFraction is only available for Expiry Futures and Perpetual Futures
51328	200	closeFraction is only available for reduceOnly orders
51329	200	closeFraction is only available in NET mode
51330	200	closeFraction is only available for stop market orders
51331	200	closeFraction is only available for close position orders
51332	200	closeFraction is not applicable to Portfolio Margin
51333	200	Close position order in hedge-mode or reduce-only order in one-way mode cannot attach TPSL
51340	200	Used margin must be greater than {0}{1}
51341	200	Position closing not allowed
51342	200	Closing order already exists. Please try again later
51343	200	TP price must be less than the lower price
51344	200	SL price must be greater than the upper price
51345	200	Policy type is not grid policy
51346	200	The highest price cannot be lower than the lowest price
51347	200	No profit available
51348	200	Stop loss price must be less than the lower price in the range.
51349	200	Take profit price must be greater than the highest price in the range.
51350	200	No recommended parameters
51351	200	Single income must be greater than 0
51352	200	You can have {0} to {1} trading pairs
51353	200	Trading pair {0} already exists
51354	200	The percentages of all trading pairs should add up to 100%
51355	200	Select a date within {0} - {1}
51356	200	Select a time within {0} - {1}
51357	200	Select a time zone within {0} - {1}
51358	200	The investment amount of each crypto must be greater than {amount}
51359	200	Recurring buy not supported for the selected crypto {0}
51370	200	The range of lever is {0}~{1}
51380	200	Market conditions do not meet the strategy running configuration. You can try again later or adjust your tp/sl configuration.
51381	200	Per grid profit ratio must be larger than 0.1% and less or equal to 10%
51382	200	Stop triggerAction is not supported by the current strategy
51383	200	The min_price is lower than the last price
51384	200	The trigger price must be greater than the min price
51385	200	The take profit price needs to be greater than the min price
51386	200	The min price needs to be greater than 1/2 of the last price
51387	200	Stop loss price must be less than the bottom price
51388	200	This Bot is in running status
51389	200	Trigger price should be lower than {0}
51390	200	Trigger price should be lower than the TP price
51391	200	Trigger price should be higher than the SL price
51392	200	TP price should be higher than the trigger price
51393	200	SL price should be lower than the trigger price
51394	200	Trigger price should be higher than the TP price
51395	200	Trigger price should be lower than the SL price
51396	200	TP price should be lower than the trigger price
51397	200	SL price should be higher than the trigger price
51398	200	Current market meets the stop condition. The bot cannot be created.
51399	200	Max margin under current leverage: {amountLimit} {quoteCurrency}. Enter a smaller amount and try again.
51400	200	Order cancellation failed as the order has been filled, canceled or does not exist.
51400	200	Cancellation failed as the order does not exist. (Only applicable to Nitro Spread)
51401	200	Cancellation failed as the order is already canceled. (Only applicable to Nitro Spread)
51402	200	Cancellation failed as the order is already completed. (Only applicable to Nitro Spread)
51403	200	Cancellation failed as the order type doesn't support cancellation.
51404	200	Order cancellation unavailable during the second phase of call auction.
51405	200	Cancellation failed as you don't have any pending orders.
51406	400	Canceled - order count exceeds the limit {param0}.
51407	200	Either order ID or client order ID is required.
51408	200	Pair ID or name doesn't match the order info.
51409	200	Either pair ID or pair name ID is required.
51410	200	Cancellation failed as the order is already in canceling status or pending settlement.
51411	200	Account does not have permission for mass cancellation.
51412	200	Cancellation timed out, please try again later.
51412	200	The order has been triggered and can't be canceled.
51413	200	Cancellation failed as the order type is not supported by endpoint.
51415	200	Unable to place order. Spot trading only supports using the last price as trigger price. Please select "Last" and try again.
51416	200	Order has been triggered and can't be canceled
51500	200	You must enter a price, quantity, or TP/SL
51501	400	Maximum {param0} orders can be modified.
51502	200	Unable to edit order: insufficient balance or margin.
51502	200	Order failed. Insufficient {param0} margin in account
51502	200	Order failed. Insufficient {param0} balance in account and Auto Borrow is not enabled
51502	200	Order failed. Insufficient {param0} margin in account and Auto Borrow is not enabled (Portfolio margin mode can try IOC orders to lower the risks)
51502	200	Order failed. The requested borrowing amount is larger than the available {param0} borrowing amount of your position tier. Existing pending orders and the new order need to borrow {param1}, remaining quota {param2}, total quota {param3}, used {param4}
51502	200	Order failed. The requested borrowing amount is larger than the available {param0} borrowing amount of your position tier. Existing pending orders and the new order need to borrow {param1}, remaining quota {param2}, total quota {param3}, used {param4}
51502	200	Order failed. The requested borrowing amount is larger than the available {param0} borrowing amount of your main account and the allocated VIP quota. Existing pending orders and the new order need to borrow {param1}, remaining quota {param2}, total quota {param3}, used {param4}
51502	200	Order failed. Insufficient available borrowing amount in {param0} crypto pair
51502	200	Order failed. Insufficient available borrowing amount in {param0} loan pool
51502	200	Order failed. Insufficient account balance and the adjusted equity in USD is smaller than the IMR.
51502	200	Order failed. The order didn't pass delta verification. If the order succeeded, the change in adjEq would be smaller than the change in IMR. Increase adjEq or reduce IMR (Portfolio margin mode can try IOC orders to lower the risks)
51503	200	Your order has already been filled or canceled.
51503	200	Order modification failed as the order does not exist. (Only applicable to Nitro Spread)
51505	200	{instId} is not in call auction
51506	200	Order modification unavailable for the order type.
51507	200	You can only place market orders for this crypto at least 5 minutes after its listing.
51508	200	Orders are not allowed to be modified during the call auction.
51509	200	Modification failed as the order has been canceled. (Only applicable to Nitro Spread)
51510	200	Modification failed as the order has been completed. (Only applicable to Nitro Spread)
51511	200	Modification failed as the order price did not meet the requirement for Post Only.
51512	200	Failed to amend orders in batches. You cannot have duplicate orders in the same amend-batch-orders request.
51513	200	Number of modification requests that are currently in progress for an order cannot exceed 3 times.
51514	200	Order modification failed. The price length must be 32 characters or shorter.
51521	200	Failed to edit. Unable to edit reduce-only order because you don't have any positions of this contract.
51522	200	Failed to edit. A reduce-only order can't be in the same trading direction as your existing positions.
51523	200	Unable to modify the order price of a stop order that closes an entire position. Please modify the trigger price instead.
51524	200	Unable to modify the order quantity of a stop order that closes an entire position. Please modify the trigger price instead.
51525	200	Stop order modification is not available for quick margin
51526	200	Order modification unsuccessful. Take profit/Stop loss conditions cannot be added to or removed from stop orders.
51527	200	Order modification unsuccessful. The stop order does not exist.
51528	200	Unable to modify trigger price type
51529	200	Order modification unsuccessful. Stop order modification only applies to Expiry Futures and Perpetual Futures.
51530	200	Order modification unsuccessful. Take profit/Stop loss conditions cannot be added to or removed from reduce-only orders.
51531	200	Order modification unsuccessful. The stop order must have either take profit or stop loss attached.
51532	200	Your TP/SL can't be modified because it was partially triggered
51536	200	Unable to modify the size of the options order if the price type is pxUsd or pxVol.
51537	200	pxUsd or pxVol are not supported by non-options instruments
51543	200	When modifying take-profit or stop-loss orders for spot or margin trading, you can only adjust the price and quantity. Cancel the order and place a new one for other actions.
51600	200	Status not found.
51601	200	Order status and order id cannot exist at the same time.
51602	200	Either order status or order ID is required.
51603	200	Order does not exist.
51604	200	Initiate a download request before obtaining the hyperlink
51605	200	You can only download transaction data from the past 2 years
51606	200	Transaction data for the current quarter is not available
51607	200	Your previous download request is still being processed
51608	200	No transaction data found for the current quarter
51610	200	You can't download billing statements for the current quarter.
51611	200	You can't download billing statements for the current quarter.
51620	200	Only affiliates can perform this action
51621	200	The user isn’t your invitee
51156	200	You're leading trades in long/short mode and can't use this API endpoint to close positions
51159	200	You're leading trades in buy/sell mode. If you want to place orders using this API endpoint, the orders must be in the same direction as your existing positions and open orders.
51162	200	You have {instrument} open orders. Cancel these orders and try again
51163	200	You hold {instrument} positions. Close these positions and try again
51165	200	The number of {instrument} reduce-only orders reached the upper limit of {upLimit}. Cancel some orders to proceed.
51166	200	Currently, we don't support leading trades with this instrument
51167	200	Failed. You have block trading open order(s), please proceed after canceling existing order(s).
51168	200	Failed. You have reduce-only type of open order(s), please proceed after canceling existing order(s)
51320	200	The range of coin percentage is {0}%-{1}%
51321	200	You're leading trades. Currently, we don't support leading trades with arbitrage, iceberg, or TWAP bots
51322	200	You're leading trades that have been filled at market price. We've canceled your open stop orders to close your positions
51323	200	You're already leading trades with take profit or stop loss settings. Cancel your existing stop orders to proceed
51324	200	As a lead trader, you hold positions in {instrument}. To close your positions, place orders in the amount that equals the available amount for closing
51325	200	As a lead trader, you must use market price when placing stop orders
51326	200	As a lead trader, you must use market price when placing orders with take profit or stop loss settings
51820	200	Request failed
51821	200	The payment method is not supported
51822	200	Quote expired
51823	200	Parameter {param} of buy/sell trading is inconsistent with the quotation
54000	200	Margin trading is not supported.
54001	200	Only Multi-currency margin account can be set to borrow coins automatically.
54004	200	Order placement or modification failed because one of the orders in the batch failed.
54005	200	Switch to isolated margin mode to trade pre-market expiry futures.
54006	200	Pre-market expiry future position limit is {posLimit}. Please cancel order or close position
54007	200	Instrument {instId} is not supported
54008	200	This operation is disabled by the 'mass cancel order' endpoint. Please enable it using this endpoint.
54009	200	The range of {param0} should be [{param1}, {param2}].
54011	200	Pre-market trading contracts are only allowed to reduce the number of positions within 1 hour before delivery. Please modify or cancel the order.
54012	200	Due to insufficient order book depth, we are now taking measures to protect your positions. Currently, you can only cancel orders, add margin to your positions, and place post-only orders. Your positions will not be liquidated. Trade will resume once order book depth returns to a safe level.
54018	200	Buy limit of {param0} USD exceeded. Your remaining limit is {param1} USD. (During the call auction)
54019	200	Buy limit of {param0} USD exceeded. Your remaining limit is {param1} USD. (After the call auction)
54024	200	Your order failed because you must enable {ccy} as collateral to trade expiry futures, perpetual futures, and options in cross-margin mode.
54025	200	Your order failed because you must enable {ccy} as collateral to trade margin, expiry futures, perpetual futures, and options in isolated margin mode.
54026	200	Your order failed because you must enable {ccy} and {ccy1} as collateral to trade the margin pair in isolated margin mode.
54027	200	Your order failed because you must enable {ccy} as collateral to trade options.
54028	200	Your order failed because you must enable {ccy} as collateral to trade spot in isolated margin mode.
54029	200	{param0} doesn’t exist within {param1}.
54030	200	Order failed. Your total value of same-direction {param0} open positions and orders can't exceed {param1} USD or {param2} of the platform's open interest.
54031	200	Order failed. The {param1} USD open position limit for {param0} has been reached.
54035	200	Order failed. The platform has reached the collateral limit for this crypto, so you can only place reduce-only orders.
54036	200	You can't place fill or kill orders when self-trade prevention is set to both maker and taker orders.
Data class
Error Code	HTTP Status Code	Error Message
52000	200	No market data found.
Spot/Margin
Error Code from 54000 to 54999

Error Code	HTTP Status Code	Error Message
54000	200	Margin trading is not supported.
54001	200	Only Multi-currency margin account can be set to borrow coins automatically.
54004	200	Order placement or modification failed because one of the orders in the batch failed.
Funding
Error Code from 58000 to 58999

Error Code	HTTP Status Code	Error Message
58002	200	Please activate Savings Account first.
58003	200	Savings does not support this currency type
58004	200	Account blocked.
58005	200	The {behavior} amount must be equal to or less than {minNum}
58006	200	Service unavailable for token {0}.
58007	200	Assets interface is currently unavailable. Try again later
58008	200	You do not have assets in this currency.
58009	200	Crypto pair doesn't exist
58010	200	Chain {chain} isn't supported
58011	200	Due to local laws and regulations, our services are unavailable to unverified users in {region}. Please verify your account.
58012	200	Due to local laws and regulations, OKX does not support asset transfers to unverified users in {region}. Please make sure your recipient has a verified account.
58013	200	Withdrawals not supported yet, contact customer support for details
58014	200	Deposits not supported yet, contact customer support for details
58015	200	Transfers not supported yet, contact customer support for details
58016	200	The API can only be accessed and used by the trading team's main account
58100	200	The trading product triggers risk control, and the platform has suspended
the fund transfer-out function with related users. Please wait patiently.
58101	200	Transfer suspended
58102	429	Rate limit reached. Please refer to API docs and throttle requests accordingly.
58103	200	This account transfer function is temporarily unavailable. Please contact customer service for details.
58104	200	Since your P2P transaction is abnormal, you are restricted from making
fund transfers. Please contact customer support to remove the restriction.
58105	200	Since your P2P transaction is abnormal, you are restricted from making
fund transfers. Please transfer funds on our website or app to complete
identity verification.
58106	200	USD verification failed.
58107	200	Crypto verification failed.
58110	200	Transfers are suspended due to market risk control triggered by your {businessType} {instFamily} trades or positions. Please try again in a few minutes. Contact customer support if further assistance is needed.
58111	200	Fund transfers are unavailable while perpetual contracts are charging funding fees. Try again later.
58112	200	Transfer failed. Contact customer support for assistance
58113	200	Unable to transfer this crypto
58114	400	Transfer amount must be greater than 0
58115	200	Sub-account does not exist.
58116	200	Transfer exceeds the available amount.
58117	200	Transfer failed. Resolve any negative assets before transferring again
58119	200	{0} Sub-account has no permission to transfer out, please set first.
58120	200	Transfers are currently unavailable. Try again later
58121	200	This transfer will result in a high-risk level of your position, which may lead to forced liquidation. You need to re-adjust the transfer amount to make sure the position is at a safe level before proceeding with the transfer.
58122	200	A portion of your spot is being used for Delta offset between positions. If the transfer amount exceeds the available amount, it may affect current spot-derivatives risk offset structure, which will result in an increased Maintenance Margin Requirement (MMR) rate. Please be aware of your risk level.
58123	200	The From parameter cannot be the same as the To parameter.
58124	200	Your transfer is being processed, transfer id:{trId}. Please check the latest state of your transfer from the endpoint (GET /api/v5/asset/transfer-state)
58125	200	Non-tradable assets can only be transferred from sub-accounts to main accounts
58126	200	Non-tradable assets can only be transferred between funding accounts
58127	200	Main account API key does not support current transfer 'type' parameter. Please refer to the API documentation.
58128	200	Sub-account API key does not support current transfer 'type' parameter. Please refer to the API documentation.
58129	200	{param} is incorrect or {param} does not match with 'type'
58131	200	For compliance, we're unable to provide services to unverified users. Verify your identity to make a transfer.
58132	200	For compliance, we're unable to provide services to users with Basic verification (Level 1). Complete Advanced verification (Level 2) to make a transfer.
58200	200	Withdrawal from {0} to {1} is currently not supported for this currency.
58201	200	Withdrawal amount exceeds daily withdrawal limit.
58202	200	The minimum withdrawal amount for NEO is 1, and the amount must be an integer.
58203	200	Please add a withdrawal address.
58204	200	Withdrawal suspended due to your account activity triggering risk control. Please contact customer support for assistance.
58205	200	Withdrawal amount exceeds the upper limit.
58206	200	Withdrawal amount is less than the lower limit.
58207	200	Withdrawal address isn't on the verified address list. (The format for withdrawal addresses with a label is “address:label”.)
58208	200	Withdrawal failed. Please link your email.
58209	200	Sub-accounts don't support withdrawals or deposits. Please use your main account instead
58210	200	You can't proceed with withdrawal as we're unable to verify your identity. Please withdraw via our app or website instead.
58212	200	Withdrawal fee must be {0}% of the withdrawal amount
58213	200	The internal transfer address is illegal. It must be an email, phone number, or account name
58214	200	Withdrawals suspended due to {chainName} maintenance
58215	200	Withdrawal ID does not exist.
58216	200	Operation not allowed.
58217	200	Withdrawals are temporarily suspended for your account due to a risk detected in your withdrawal address. Contact customer support for assistance
58218	200	The internal withdrawal failed. Please check the parameters toAddr and areaCode.
58219	200	You cannot withdraw crypto within 24 hours after changing your mobile number, email address, or Google Authenticator.
58220	200	Withdrawal request already canceled.
58221	200	The toAddr parameter format is incorrect, withdrawal address needs labels. The format should be "address:label".
58222	200	Invalid withdrawal address
58223	200	This is a contract address with higher withdrawal fees
58224	200	This crypto currently doesn't support on-chain withdrawals to OKX addresses. Withdraw through internal transfers instead
58225	200	Asset transfers to unverified users in {region} are not supported due to local laws and regulations.
58226	200	{chainName} is delisted and not available for crypto withdrawal.
58227	200	Withdrawal of non-tradable assets can be withdrawn all at once only
58228	200	Withdrawal of non-tradable assets requires that the API key must be bound to an IP
58229	200	Insufficient funding account balance to pay fees {fee} USDT
58230	200	According to the OKX compliance policy, you will need to complete your identity verification (Level 1) in order to withdraw
58231	200	The recipient has not completed personal info verification (Level 1) and cannot receive your transfer
58232	200	You’ve reached the personal information verification (L1) withdrawal limit, complete photo verification (L2) to increase the withdrawal limit
58233	200	For compliance, we're unable to provide services to unverified users. Verify your identity to withdraw.
58234	200	For compliance, the recipient can't receive your transfer yet. They'll need to verify their identity to receive your transfer.
58235	200	For compliance, we're unable to provide services to users with Basic verification (Level 1). Complete Advanced verification (Level 2) to withdraw.
58236	200	For compliance, a recipient with Basic verification (Level 1) is unable to receive your transfer. They'll need to complete Advanced verification (Level 2) to receive it.
58237	200	According to local laws and regulations, please provide accurate recipient information (rcvrInfo). For the exchange address, please also provide exchange information and recipient identity information ({consientParameters}).
58238	200	Incomplete info. The info of the exchange and the recipient are required if you're withdrawing to an exchange platform.
58239	200	You can't withdraw to a private wallet via API. Please withdraw via our app or website instead.
58240	200	For security and compliance purposes, please complete the identity verification process to use our services. If you prefer not to verify, contact customer support for next steps. We're committed to ensuring a safe platform for users and appreciate your understanding.
58241	200	Due to local compliance requirements, internal withdrawal is unavailable
58242	200	The recipient can't receive your transfer due to their local compliance requirements
58243	200	Your recipient can't receive your transfer as they haven't made a cash deposit yet
58244	200	Make a cash deposit to proceed
58248	200	Due to local regulations, API withdrawal isn't allowed. Withdraw using OKX app or web.
58249	200	API withdrawal for this currency is currently unavailable. Try withdrawing via our app or website.
58252	200	Withdrawal is restricted for 48h after your first TRY transaction for asset security.
58300	200	Deposit-address count exceeds the limit.
58301	200	Deposit-address not exist.
58302	200	Deposit-address needs tag.
58303	200	Deposit for chain {chain} is currently unavailable
58304	200	Failed to create invoice.
58305	200	Unable to retrieve deposit address, please complete identity verification and generate deposit address first.
58306	200	According to the OKX compliance policy, you will need to complete your identity verification (Level 1) in order to deposit
58307	200	You've reached the personal information verification (L1) deposit limit, the excess amount has been frozen, complete photo verification (L2) to increase the deposit limit
58308	200	For compliance, we're unable to provide services to unverified users. Verify your identity to deposit.
58309	200	For compliance, we're unable to provide services to users with Basic verification (Level 1). Complete Advanced verification (Level 2) to deposit.
58310	200	Unable to create new deposit address, try again later
58350	200	Insufficient balance.
58351	200	Invoice expired.
58352	200	Invalid invoice.
58353	200	Deposit amount must be within limits.
58354	200	You have reached the daily limit of 10,000 invoices.
58355	200	Permission denied. Please contact your account manager.
58356	200	The accounts of the same node do not support the Lightning network deposit or withdrawal.
58358	200	The fromCcy parameter cannot be the same as the toCcy parameter.
58373	200	The minimum {ccy} conversion amount is {amount}
58400	200	Request Failed
58401	200	Payment method is not supported
58402	200	Invalid payment account
58403	200	Transaction cannot be canceled
58404	200	ClientId already exists
58405	200	Withdrawal suspended
58406	200	Channel is not supported
58407	200	API withdrawal isn't allowed for this payment method. Withdraw using OKX app or web
Account
Error Code from 59000 to 59999

Error Code	HTTP Status Code	Error Message
59000	200	Settings failed. Close any open positions or orders before modifying settings.
59001	200	Switching unavailable as you have borrowings.
59002	200	Sub-account settings failed. Close any open positions, orders, or trading bots before modifying settings.
59004	200	Only IDs with the same instrument type are supported
59005	200	When margin is manually transferred in isolated mode, the value of the asset intially allocated to the position must be greater than 10,000 USDT.
59006	200	This feature is unavailable and will go offline soon.
59101	200	Leverage can't be modified. Please cancel all pending isolated margin orders before adjusting the leverage.
59102	200	Leverage exceeds the maximum limit. Please lower the leverage.
59103	200	Account margin is insufficient and leverage is too low. Please increase the leverage.
59104	200	The borrowed position has exceeded the maximum position of this leverage. Please lower the leverage.
59105	400	Leverage can't be less than {0}. Please increase the leverage.
59106	200	The max available margin corresponding to your order tier is {param0}. Please adjust your margin and place a new order.
59107	200	Leverage can't be modified. Please cancel all pending cross-margin orders before adjusting the leverage.
59108	200	Your account leverage is too low and has insufficient margins. Please increase the leverage.
59109	200	Account equity less than the required margin amount after adjustment. Please adjust the leverage.
59110	200	The instrument corresponding to this {param0} does not support the tgtCcy parameter.
59111	200	Leverage query isn't supported in portfolio margin account mode
59112	200	You have isolated/cross pending orders. Please cancel them before adjusting your leverage
59113	200	According to local laws and regulations, margin trading service is not available in your region. If your citizenship is at a different region, please complete KYC2 verification.
59114	200	According to local laws and regulations, margin trading services are not available in your region.
59117	200	Cannot select more than {param0} crypto types
59118	200	Amount placed should greater than {param0}
59119	200	One-click repay is temporarily unavailable. Try again later.
59120	200	One-click convert is temporarily unavailable. Try again later.
59121	200	This batch is still under processing, please wait patiently.
59122	200	This batch has been processed
59123	200	{param0} order amount must be greater than {param1}
59124	200	The order amount of {param0} must be less than {param1}.
59125	200	{param0} doesn't support the current operation.
59132	200	Unable to switch. Please close or cancel all open orders and refer to the pre-check endpoint to stop any incompatible bots.
59133	200	Unable to switch due to insufficient assets for the chosen account mode.
59134	200	Unable to switch. Refer to the pre-check endpoint and close any incompatible positions.
59135	200	Unable to switch. Refer to the pre-check endpoint and adjust your trades from copy trading.
59136	200	Unable to switch. Pre-set leverage for all cross margin contract positions then try again.
59137	200	Lower leverage to {param0} or below for all cross margin contract positions and try again.
59138	200	Unable to switch due to a position tier check failure.
59139	200	Unable to switch due to a margin check failure.
59140	200	You can only repay with your collateral crypto.
59141	200	The minimum repayment amount is {param0}. Select more available crypto or increase your trading account balance.
59142	200	Instant repay failed. You can only repay borrowable crypto.
59200	200	Insufficient account balance.
59201	200	Negative account balance.
59202	200	No access to max opening amount in cross positions for PM accounts.
59300	200	Margin call failed. Position does not exist.
59301	200	Margin adjustment failed for exceeding the max limit.
59302	200	Margin adjustment failed due to pending close order. Please cancel any pending close orders.
59303	200	Insufficient available margin, add margin or reduce the borrowing amount
59304	200	Insufficient equity for borrowing. Keep enough funds to pay interest for at least one day.
59305	200	Use VIP loan first to set the VIP loan priority
59306	200	Your borrowing amount exceeds the max limit
59307	200	You are not eligible for VIP loans
59308	200	Unable to repay VIP loan due to insufficient borrow limit
59309	200	Unable to repay an amount that exceeds the borrowed amount
59310	200	Your account does not support VIP loan
59311	200	Setup cannot continue. An outstanding VIP loan exists.
59312	200	{currency} does not support VIP loans
59313	200	Unable to repay. You haven't borrowed any ${ccy} (${ccyPair}) in Quick margin mode.
59314	200	The current user is not allowed to return the money because the order is not borrowed
59315	200	viploan is upgrade now. Wait for 10 minutes and try again
59316	200	The current user is not allowed to borrow coins because the currency is in the order in the currency borrowing application.
59317	200	The number of pending orders that are using VIP loan for a single currency cannot be more than {maxNumber} (orders)
59319	200	You can’t repay your loan order because your funds are in use. Make them available for full repayment.
59401	200	Holdings limit reached.
59402	200	No passed instIDs are in a live state. Please verify instIDs separately.
59410	200	You can only borrow this crypto if it supports borrowing and borrowing is enabled.
59411	200	Manual borrowing failed. Your account's free margin is insufficient.
59412	200	Manual borrowing failed. The amount exceeds your borrowing limit.
59413	200	You didn't borrow this crypto. No repayment needed.
59414	200	Manual borrowing failed. The minimum borrowing limit is {param0}.
59500	200	Only the API key of the main account has permission.
59501	200	Each account can create up to 50 API keys
59502	200	This note name already exists. Enter a unique API key note name
59503	200	Each API key can bind up to 20 IP addresses
59504	200	Sub-accounts don't support withdrawals. Please use your main account for withdrawals.
59505	200	The passphrase format is incorrect.
59506	200	API key doesn't exist.
59507	200	The two accounts involved in a transfer must be 2 different sub-accounts under the same main account.
59508	200	The sub account of {param0} is suspended.
59509	200	Account doesn't have permission to reset market maker protection (MMP) status.
59510	200	Sub-account does not exist
59512	200	Unable to set up permissions for ND broker subaccounts. By default, all ND subaccounts can transfer funds out.
59515	200	You are currently not on the custody whitelist. Please contact customer service for assistance.
59516	200	Please create the Copper custody funding account first.
59517	200	Please create the Komainu custody funding account first.
59518	200	You can’t create a sub-account using the API; please use the app or web.
59519	200	You can’t use this function/feature while it's frozen, due to: {freezereason}
59601	200	Subaccount name already exists.
59603	200	Maximum number of subaccounts reached.
59604	200	Only the API key of the main account can access this API.
59606	200	Failed to delete sub-account. Transfer all sub-account funds to your main account before deleting your sub-account.
59608	200	Only Broker accounts have permission to access this API.
59609	200	Broker already exists
59610	200	Broker does not exist
59611	200	Broker unverified
59612	200	Cannot convert time format
59613	200	No escrow relationship established with the subaccount.
59614	200	Managed subaccount does not support this operation.
59615	200	The time interval between the Begin Date and End Date cannot be greater than 180 days.
59616	200	The Begin Date cannot be later than the End Date.
59617	200	Sub-account created. Account level setup failed.
59618	200	Failed to create sub-account.
59619	200	This endpoint does not support ND sub accounts. Please use the dedicated endpoint supported for ND brokers.
59622	200	You're creating a sub-account for a non-existing or incorrect sub-account. Create a sub-account under the ND broker first or use the correct sub-account code.
59623	200	Couldn't delete the sub-account under the ND broker as the sub-account has one or more sub-accounts, which must be deleted first.
59648	200	Your modified spot-in-use amount is insufficient, which may lead to liquidation. Adjust the amount.
59649	200	Disabling spot-derivatives risk offset mode may increase the risk of liquidation. Adjust the size of your positions and ensure your maintenance maintenance margin ratio is safe.
59650	200	Switching your offset unit may increase the risk of liquidation. Adjust the size of your positions and ensure your maintenance maintenance margin ratio is safe.
59651	200	Enable spot-derivatives risk offset mode to set your spot-in-use amount.
59652	200	You can only set a spot-in-use amount for crypto that can be used as margin.
59658	200	{ccy} isn’t supported as collateral.
59658	200	{ccy} and {ccy1} aren’t supported as collateral.
59658	200	{ccy}, {ccy1}, and {ccy2} aren’t supported as collateral.
59658	200	{ccy}, {ccy1}, {ccy2}, and {number} other crypto aren’t supported as collateral.
59659	200	Failed to apply settings because you must also enable {ccy} to enable {ccy1} as collateral.
59660	200	Failed to apply settings because you must also disable {ccy} to disable {ccy1} as collateral.
59661	200	Failed to apply settings because you can’t disable {ccy} as collateral.
59662	200	Failed to apply settings because of open orders or positions requiring {ccy} as collateral.
59662	200	Failed to apply settings because of open orders or positions requiring {ccy} and {ccy1} as collateral.
59662	200	Failed to apply settings because of open orders or positions requiring {ccy}, {ccy1}, and {ccy2} as collateral.
59662	200	Failed to apply settings because of open orders or positions requiring {ccy}, {ccy1}, {ccy2}, and {number} other crypto as collateral.
59664	200	Failed to apply settings because you have borrowings in {ccy}.
59664	200	Failed to apply settings because you have borrowings in {ccy} and {ccy1}.
59664	200	Failed to apply settings because you have borrowings in {ccy}, {ccy1}, and {ccy2}.
59664	200	Failed to apply settings because you have borrowings in {ccy}, {ccy1}, {ccy2}, and {number} other crypto.
59665	200	Failed to apply settings. Enable other cryptocurrencies as collateral to meet the position’s margin requirements.
59666	200	Failed to apply settings because you can’t enable and disable a crypto as collateral at the same time.
59668	200	Cancel isolated margin TP/SL, trailing, trigger, and chase orders or stop bots before adjusting your leverage.
59669	200	Cancel cross-margin TP/SL, trailing, trigger, and chase orders or stop bots before adjusting your leverage.
59670	200	You have more than {param0} open orders for this trading pair. Cancel to reduce your orders to {param1} or fewer before adjusting your leverage.
59671	200	Auto-earn currently doesn’t support {param0}.
59672	200	You can’t modify your minimmum lending APR when Auto-earn is off.
59673	200	You can’t turn off Auto-earn within 24 hours of turning it on. Try again at {param0}.
59674	200	You can’t borrow to transfer or withdraw when Auto-earn is on for this cryptocurrency.
59675	200	You’ve already turned on Auto-earn for {param0}.
59676	200	You can only use Auto-earn if your trading fee tier is {param0} or higher.
WebSocket
Public
Error Code from 60000 to 64002

General Class
Error Code	Error Message
60004	Invalid timestamp
60005	Invalid apiKey
60006	Timestamp request expired
60007	Invalid sign
60008	The current WebSocket endpoint does not support subscribing to {0} channels. Please check the WebSocket URL
60009	Login failure
60011	Please log in
60012	Invalid request
60013	Invalid args
60014	Requests too frequent
60018	Wrong URL or {0} doesn't exist. Please use the correct URL, channel and parameters referring to API document.
60019	Invalid op: {op}
60023	Bulk login requests too frequent
60024	Wrong passphrase
60026	Batch login by APIKey and token simultaneously is not supported.
60027	Parameter {0} can not be empty.
60028	The current operation is not supported by this URL. Please use the correct WebSocket URL for the operation.
60029	Only users who are VIP5 and above in trading fee tier are allowed to subscribe to this channel.
60030	Only users who are VIP4 and above in trading fee tier are allowed to subscribe to books50-l2-tbt channel.
60031	The WebSocket endpoint does not allow multiple or repeated logins.
60032	API key doesn't exist.
63999	Login failed due to internal error. Please try again later.
64000	Subscription parameter uly is unavailable anymore, please replace uly with instFamily. More details can refer to: https://www.okx.com/help-center/changes-to-v5-api-websocket-subscription-parameter-and-url.
64001	This channel has been migrated to the '/business' URL. Please subscribe using the new URL. More details can refer to: https://www.okx.com/help-center/changes-to-v5-api-websocket-subscription-parameter-and-url.
64002	This channel is not supported by "/business" URL. Please use "/private" URL(for private channels), or "/public" URL(for public channels). More details can refer to: https://www.okx.com/help-center/changes-to-v5-api-websocket-subscription-parameter-and-url.
64003	Your trading fee tier doesn't meet the requirement to access this channel
64004	Subscribe to both {channelName} and books-l2-tbt for {instId} is not allowed. Unsubscribe books-l2-tbt first.
64007	Operation {0} failed due to WebSocket internal error. Please try again later.
64008	The connection will soon be closed for a service upgrade. Please reconnect.
Close Frame
Status Code	Reason Text
1009	Request message exceeds the maximum frame length
4001	Login Failed
4002	Invalid Request
4003	APIKey subscription amount exceeds the limit 100
4004	No data received in 30s
4005	Buffer is full, cannot write data
4006	Abnormal disconnection
4007	API key has been updated or deleted. Please reconnect.
4008	The number of subscribed channels exceeds the maximum limit.
4009	The number of subscription channels for this connection exceeds the limit
