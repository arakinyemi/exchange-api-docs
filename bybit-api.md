# Introduction

## Overview
The V5 API brings uniformity and efficiency to Bybit's product lines, unifying Spot, Derivatives, and Options in one set of specifications.

## Current API Coverage

| OpenAPI Version | Account Type             | Linear | Inverse | Spot | Options | USDT Perpetual | USDC Perpetual | USDC Futures | Perpetual | Futures |
|-----------------|--------------------------|--------|---------|------|---------|----------------|----------------|--------------|-----------|---------|
| V5              | Unified trading account  | ✓      | ✓       | ✓    | ✓       | ✓              | ✓              | ✓            | ✓         | ✓       |
| V5              | Classic account          | ✓      |         | ✓    | ✓       | ✓              |                |              |           |         |

## Key Upgrades

### Product Lines Alignment
V5 unifies the APIs of various trading products into one, providing users the capability to trade Spot, Derivatives and Options contracts with a single API by distinguishing transactions through different order parameters. Consequently, there is no need to switch between multiple interfaces when building different products – regardless of tasks such as order management or querying wallet data – as the same API can be used to request and return results.

The global dictionary in V5 is uniquely defined to avoid scenarios where different terms are used for the same purposes, or where a single term has multiple references. This simplifies the troubleshooting process for users.

For example, when creating an order with `POST V5/order/create`, users can specify `cate  ry=spot/linear/inverse/option` to create multiple orders across different products.

### Enhance Capital Efficiency
V5 provides users the ability to upgrade accounts to a unified account, allowing for sharing and cross-utilization of funds across Spot, USDT Perpetual, USDC Perpetual and Options contracts. Users are also able to offset profit and losses across different positions, thus further enhancing capital efficiency.

### Unified Account Borrowing
API V5 supports borrowing across a unified account mode. Users can pledge multiple assets as collateral to obtain margin for trading across different products.

**Example:** Trader Alice opens a USDT-settled BTCUSDT contract position while holding only BTC assets. If a floating loss is incurred, a debt will be recorded and interest will be charged hourly.

### Portfolio Margin Mode
Unified Accounts now support combined margin between Inverse Perpetuals, Inverse Futures, USDT Perpetual, USDC Perpetual, USDC Futures and Options.

### API Interface Naming Standard
API V5 offers clearer path definitions for improved clarity and reduced ambiguity. The new version divides the API path into market data, order management, position management, account management, asset management, and more modules.

```

{host}/{version}/{product}/{module}

```

**Example:** `api.bybit.com/v5/market/recent-trade`

| Address Segment             | Description                                                                                                        |
|-----------------------------|--------------------------------------------------------------------------------------------------------------------|
| v5/market/                  | Candlestick, orderbook, ticker, platform transaction data, underlying financial rules, risk control rules         |
| v5/order/                   | Order management                                                                                                   |
| v5/position/                | Position management                                                                                                |
| v5/account/                 | Single account operations only – unified funding account, rates, etc.                                             |
| v5/asset/                   | Operations across multiple accounts – asset management, fund management, etc.                                     |
| v5/spot-lever-token/        | Obtain quotes from Leveraged Tokens on Spot, and to exercise purchase and redeem functions                         |
| v5/spot-margin-trade/       | Manage Margin Trading on Spot                                                                                      |

### Cancellation of Orders by Settlement Currency
Users can cancel all Derivatives orders settled by the same currency with `settleCoin`.

### Addition of Insurance Fund Interface
This interface addition allows for queries of the insurance pool, which users can use to check for any insurance fund updates on the Bybit platform.

### Readability Improvements to API Documentation
The API documentation has been revised and proofread to improve clarity and reduce confusion. Any parts of the previous documentation that were not clear have been revised to provide better explanations.

---

## Integration Guidance
> To learn more about the V5 API, please read the Introduction.

---

## API Resources and Support Channels
- 📌 Help Center
- 🎉 Official   SDK
- 🎉 Official    SDK
- 🎉 Official    SDK
- 🎉 Official .Net SDK
- 🎉 Community    SDK
- ✉️ Telegram - API Discussion Group
- ✉️ Discord
- 💡 Postman collection
- 💡 API usage examples

---

## Authentication
> Please visit Bybit's testnet or mainnet to generate an API key

### REST API Base Endpoint:

**Testnet:**  
`https://api-testnet.bybit.com`

**Mainnet (both endpoints are available):**  
`https://api.bybit.com`  
`https://api.bytick.com`

**Important:**
- Netherlands users: use `https://api.bybit.nl` for mainnet
- Turkey users: use `https://api.bybit-tr.com` for mainnet
- Kazakhstan users: use `https://api.bybit.kz` for mainnet
- Georgia users: use `https://api.bybitgeorgia.ge` for mainnet
- ARE users: use `https://api.bybit.ae` for mainnet

---

### Select Your API Key Type

#### System-generated API Keys
The API key generated by the Bybit system operates with HMAC encryption. You will be provided with a pair of public and private keys. Please treat this pair of keys as passwords and keep them safe.

Follow HMAC sample scripts to complete encryption procedures.

#### Auto-generated API Keys
Self-generated API keys operate with RSA encryption. You must create your public and private keys through the software, and then only provide the public key to Bybit, we will never hold your private key.

Use `api-rsa-generator` to create RSA private and public keys  
Follow the RSA sample scripts to complete encryption procedures.

---

## HTTP Headers for Authenticated Endpoints
The following HTTP header keys must be used for authentication:

- `X-BAPI-API-KEY` - API key
- `X-BAPI-TIMESTAMP` - UTC timestamp in milliseconds
- `X-BAPI-SIGN` - a signature derived from the request's parameters
- `X-Referer` or `Referer` - the header for broker users only

We also provide `X-BAPI-RECV-WINDOW` (unit in millisecond and default value is 5,000) to specify how long an HTTP request is valid. It is also used to prevent replay attacks.

A smaller `X-BAPI-RECV-WINDOW` is more secure, but your request may fail if the transmission time is greater than your `X-BAPI-RECV-WINDOW`.

---

### Caution
Please make sure that the timestamp parameter adheres to the following rule:

```

server\_time - recv\_window <= timestamp < server\_time + 1000

```

which means your timestamp should lie in range:

```

\[server\_time - recv\_window; server\_time + 1000)

```

**Note:**  
`server_time` stands for Bybit server time, which can be queried via the Server Time endpoint. Keep in mind it's highly recommended that you use local device time for timestamp and keep it NTP-synchronized at all times.

---

## Create A Request
> To assist in diagnosing advanced network problems, you may consider adding `cdn-request-id` to your request headers. Its value should be unique for each request.

**Basic steps:**
1. Calculate the string you want to sign as follows:  
   - For **GET** requests:  
     `timestamp + API key + recv_window + queryString`  
   - For **POST** requests:  
     `timestamp + API key + recv_window + jsonBodyString`
2. Use the `HMAC_SHA256` or `RSA_SHA256` al  rithm to sign the string in step 1, and convert it to:  
   - lowercase HEX string for HMAC_SHA256  
   - base64 for RSA_SHA256  
   to obtain the string value of your signature.
3. Add your signature to `X-BAPI-API-KEY` header and send the HTTP request.

**Example:**  
Example signature al  rithms can be found here.

---

### Example for generating plain text to encrypt

**GET**  
**POST**

**Rule:**  
```

timestamp + api\_key + recv\_window + queryString

```

**Example values:**
```

timestamp = "1658384314791"
api\_key = "XXXXXXXXXX"
recv\_window = "5000"
queryString = "cate  ry=option\&symbol=BTC-29JUL22-25000-C"

```

**Resulting string that needs to be signed:**
```

1658384314791XXXXXXXXXX5000cate  ry=option\&symbol=BTC-29JUL22-25000-C

```

**Resulting example signature for HMAC:**
```

410e0f387bafb7afd0f1722c068515e09945610124fa11774da1da857b72f30b

```

---

## HTTP request examples

### GET


GET /v5/order/realtime?cate  ry=option\&symbol=BTC-29JUL22-25000-C HTTP/1.1
Host: api-testnet.bybit.com
-H 'X-BAPI-SIGN: XXXXXXXXXX'&#x20;
-H 'X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx'&#x20;
-H 'X-BAPI-TIMESTAMP: 1658384431891'&#x20;
-H 'X-BAPI-RECV-WINDOW: 5000'



## Common response parameters

| Parameter   | Type   | Comments                                                                 |
|-------------|--------|--------------------------------------------------------------------------|
| retCode     | number | Success/Error code                                                       |
| retMsg      | string | Success/Error msg. OK, success, SUCCESS, "" indicates a successful response |
| result      | Object | Business data result                                                     |
| retExtInfo  | Object | Extend info. Most of the time it is {}                                   |
| time        | number | Current timestamp (ms)                                                   |

**Example:**

{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
    },
    "retExtInfo": {},
    "time": 1671017382656
}






Get System Status
Get the system status when there is a platform maintenance or service incident.

info
Please note currently system maintenance that may result in short interruption (lasting less than 10 seconds) or websocket disconnection (users can immediately reconnect) will not be announced.

HTTP Request
GET /v5/system/status

Request Parameters
Parameter	Required	Type	Comments
id	false	string	id. Unique identifier
state	false	string	system state
Response Parameters
Parameter	Type	Comments
list	array	Object
> id	string	Id. Unique identifier
> title	string	Title of system maintenance
> state	string	System state
> begin	string	Start time of system maintenance, timestamp in milliseconds
> end	string	End time of system maintenance, timestamp in milliseconds. Before maintenance is completed, it is the expected end time; After maintenance is completed, it will be changed to the actual end time.
> href	string	Hyperlink to system maintenance details. Default value is empty string
> serviceTypes	array<int>	Service Type
> product	array<int>	Product
> uidSuffix	array<int>	Affected UID tail number
> maintainType	string	Maintenance type
> env	string	Environment
Request Example
GET /v5/system/status HTTP/1.1
Host: api.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "list": [
            {
                "id": "4d95b2a0-587f-11f0-bcc9-56f28c94d6ea",
                "title": "t06",
                "state": "completed",
                "begin": "1751596902000",
                "end": "1751597011000",
                "href": "",
                "serviceTypes": [
                    2,
                    3,
                    4,
                    5
                ],
                "product": [
                    1,
                    2
                ],
                "uidSuffix": [],
                "maintainType": 1,
                "env": 1
            },
            {
                "id": "19bb6f82-587f-11f0-bcc9-56f28c94d6ea",
                "title": "t05",
                "state": "completed",
                "begin": "1751254200000",
                "end": "1751254500000",
                "href": "",
                "serviceTypes": [
                    1,
                    4
                ],
                "product": [
                    1
                ],
                "uidSuffix": [],
                "maintainType": 3,
                "env": 1
            },
            {
                "id": "25f4bc8c-533c-11f0-bcc9-56f28c94d6ea",
                "title": "t04",
                "state": "completed",
                "begin": "1751017967000",
                "end": "1751018096000",
                "href": "",
                "serviceTypes": [
                    2
                ],
                "product": [
                    2
                ],
                "uidSuffix": [],
                "maintainType": 1,
                "env": 1
            },
            {
                "id": "679a9c5f-533b-11f0-bcc9-56f28c94d6ea",
                "title": "t03",
                "state": "completed",
                "begin": "1751017532000",
                "end": "1751017658000",
                "href": "",
                "serviceTypes": [
                    5,
                    4
                ],
                "product": [
                    1,
                    2
                ],
                "uidSuffix": [],
                "maintainType": 2,
                "env": 1
            },
            {
                "id": "c8990f96-5332-11f0-8fd3-c241b123dd9e",
                "title": "t02",
                "state": "completed",
                "begin": "1751013817000",
                "end": "1751013890000",
                "href": "",
                "serviceTypes": [
                    5,
                    4,
                    3,
                    2,
                    1
                ],
                "product": [
                    4,
                    3,
                    2,
                    1
                ],
                "uidSuffix": [],
                "maintainType": 2,
                "env": 1
            },
            {
                "id": "f9d6842d-5331-11f0-8fd3-c241b123dd9e",
                "title": "t01",
                "state": "completed",
                "begin": "1751012688000",
                "end": "1751012760000",
                "href": "",
                "serviceTypes": [
                    1,
                    2,
                    3,
                    4,
                    5
                ],
                "product": [
                    1,
                    2,
                    3,
                    4
                ],
                "uidSuffix": [],
                "maintainType": 3,
                "env": 2
            }
        ]
    },
    "retExtInfo": {},
    "time": 1751858399649
}



Get Bybit Server Time
HTTP Request
GET /v5/market/time

Request Parameters
None

Response Parameters
Parameter	Type	Comments
timeSecond	string	Bybit server timestamp (sec)
timeNano	string	Bybit server timestamp (nano)
RUN >>
Request Example
HTTP
GET /v5/market/time HTTP/1.1
Host: api.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "timeSecond": "1688639403",
        "timeNano": "1688639403423213947"
    },
    "retExtInfo": {},
    "time": 1688639403423
}

Get Kline
Query for historical klines (also known as candles/candlesticks). Charts are returned in groups based on the requested interval.

Covers: Spot / USDT contract / USDC contract / Inverse contract

HTTP Request
GET /v5/market/kline

Request Parameters
Parameter	Required	Type	Comments
cate  ry	false	string	Product type. spot,linear,inverse
When cate  ry is not passed, use linear by default
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
interval	true	string	Kline interval. 1,3,5,15,30,60,120,240,360,720,D,W,M
start	false	integer	The start timestamp (ms)
end	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 1000]. Default: 200
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
symbol	string	Symbol name
list	array	
An string array of individual candle
Sort in reverse by startTime
> list[0]: startTime	string	Start time of the candle (ms)
> list[1]: openPrice	string	Open price
> list[2]: highPrice	string	Highest price
> list[3]: lowPrice	string	Lowest price
> list[4]: closePrice	string	Close price. Is the last traded price when the candle is not closed
> list[5]: volume	string	Trade volume
USDT or USDC contract: unit is base coin (e.g., BTC)
Inverse contract: unit is quote coin (e.g., USD)
> list[6]: turnover	string	Turnover.
USDT or USDC contract: unit is quote coin (e.g., USDT)
Inverse contract: unit is base coin (e.g., BTC)
RUN >>
Request Example
HTTP
GET /v5/market/kline?cate  ry=inverse&symbol=BTCUSD&interval=60&start=1670601600000&end=1670608800000 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "symbol": "BTCUSD",
        "cate  ry": "inverse",
        "list": [
            [
                "1670608800000",
                "17071",
                "17073",
                "17027",
                "17055.5",
                "268611",
                "15.74462667"
            ],
            [
                "1670605200000",
                "17071.5",
                "17071.5",
                "17061",
                "17071",
                "4177",
                "0.24469757"
            ],
            [
                "1670601600000",
                "17086.5",
                "17088",
                "16978",
                "17071.5",
                "6356",
                "0.37288112"
            ]
        ]
    },
    "retExtInfo": {},
    "time": 1672025956592
}

Get Mark Price Kline
Query for historical mark price klines. Charts are returned in groups based on the requested interval.

Covers: USDT contract / USDC contract / Inverse contract

HTTP Request
GET /v5/market/mark-price-kline

Request Parameters
Parameter	Required	Type	Comments
cate  ry	false	string	Product type. linear,inverse
When cate  ry is not passed, use linear by default
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
interval	true	string	Kline interval. 1,3,5,15,30,60,120,240,360,720,D,M,W
start	false	integer	The start timestamp (ms)
end	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 1000]. Default: 200
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
symbol	string	Symbol name
list	array	
An string array of individual candle
Sort in reverse by startTime
> list[0]: startTime	string	Start time of the candle (ms)
> list[1]: openPrice	string	Open price
> list[2]: highPrice	string	Highest price
> list[3]: lowPrice	string	Lowest price
> list[4]: closePrice	string	Close price. Is the last traded price when the candle is not closed
RUN >>
Request Example
HTTP
 
  
  
  
GET /v5/market/mark-price-kline?cate  ry=linear&symbol=BTCUSDT&interval=15&start=1670601600000&end=1670608800000&limit=1 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "symbol": "BTCUSDT",
        "cate  ry": "linear",
        "list": [
            [
            "1670608800000",
            "17164.16",
            "17164.16",
            "17121.5",
            "17131.64"
            ]
        ]
    },
    "retExtInfo": {},
    "time": 1672026361839
}

Get Index Price Kline
Query for historical index price klines. Charts are returned in groups based on the requested interval.

Covers: USDT contract / USDC contract / Inverse contract

HTTP Request
GET /v5/market/index-price-kline

Request Parameters
Parameter	Required	Type	Comments
cate  ry	false	string	Product type. linear,inverse
When cate  ry is not passed, use linear by default
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
interval	true	string	Kline interval. 1,3,5,15,30,60,120,240,360,720,D,W,M
start	false	integer	The start timestamp (ms)
end	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 1000]. Default: 200
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
symbol	string	Symbol name
list	array	
An string array of individual candle
Sort in reverse by startTime
> list[0]: startTime	string	Start time of the candle (ms)
> list[1]: openPrice	string	Open price
> list[2]: highPrice	string	Highest price
> list[3]: lowPrice	string	Lowest price
> list[4]: closePrice	string	Close price. Is the last traded price when the candle is not closed

Request Example

GET /v5/market/index-price-kline?cate  ry=inverse&symbol=BTCUSDZ22&interval=1&start=1670601600000&end=1670608800000&limit=2 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "symbol": "BTCUSDZ22",
        "cate  ry": "inverse",
        "list": [
            [
                "1670608800000",
                "17167.00",
                "17167.00",
                "17161.90",
                "17163.07"
            ],
            [
                "1670608740000",
                "17166.54",
                "17167.69",
                "17165.42",
                "17167.00"
            ]
        ]
    },
    "retExtInfo": {},
    "time": 1672026471128
}

Get Premium Index Price Kline
Query for historical premium index klines. Charts are returned in groups based on the requested interval.

Covers: USDT and USDC perpetual

HTTP Request
GET /v5/market/premium-index-price-kline

Request Parameters
Parameter	Required	Type	Comments
cate  ry	false	string	Product type. linear
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
interval	true	string	Kline interval. 1,3,5,15,30,60,120,240,360,720,D,W,M
start	false	integer	The start timestamp (ms)
end	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 1000]. Default: 200
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
symbol	string	Symbol name
list	array	
An string array of individual candle
Sort in reverse by start
> list[0]	string	Start time of the candle (ms)
> list[1]	string	Open price
> list[2]	string	Highest price
> list[3]	string	Lowest price
> list[4]	string	Close price. Is the last traded price when the candle is not closed
RUN >>
Request Example
HTTP
 
  
  
  
GET /v5/market/premium-index-price-kline?cate  ry=linear&symbol=BTCUSDT&interval=D&start=1652112000000&end=1652544000000 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "symbol": "BTCUSDT",
        "cate  ry": "linear",
        "list": [
            [
                "1652486400000",
                "-0.000587",
                "-0.000344",
                "-0.000480",
                "-0.000344"
            ],
            [
                "1652400000000",
                "-0.000989",
                "-0.000561",
                "-0.000587",
                "-0.000587"
            ]
        ]
    },
    "retExtInfo": {},
    "time": 1672765216291
}

Get Instruments Info
Query for the instrument specification of online trading pairs.

Covers: Spot / USDT contract / USDC contract / Inverse contract / Option

info
Spot does not support pagination, so limit, cursor are invalid.
When querying by baseCoin, regardless of if cate  ry=linear or inverse, the result will contain USDT contract, USDC contract and Inverse contract symbols.
caution
This endpoint returns 500 entries by default. There are now more than 500 linear symbols on the platform. As a result, you will need to use cursor for pagination or limit to get all entries.

HTTP Request
GET /v5/market/instruments-info

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. spot,linear,inverse,option
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
status	false	string	Symbol status filter
By default returns only Trading symbols
Spot has Trading only
linear & inverse: when status=PreLaunch, it returns Pre-Market contracts
baseCoin	false	string	Base coin, uppercase only
Applies to linear,inverse,option only
option: returns BTC by default
limit	false	integer	Limit for data size per page. [1, 1000]. Default: 500
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Linear/Inverse
Option
Spot
Parameter	Type	Comments
cate  ry	string	Product type
nextPageCursor	string	Cursor. Used to pagination
list	array	Object
> symbol	string	Symbol name
> contractType	string	Contract type
> status	string	Instrument status
> baseCoin	string	Base coin
> quoteCoin	string	Quote coin
> launchTime	string	Launch timestamp (ms)
> deliveryTime	string	Delivery timestamp (ms)
Expired futures delivery time
Perpetual delisting time
> deliveryFeeRate	string	Delivery fee rate
> priceScale	string	Price scale
> leverageFilter	Object	Leverage attributes
>> minLeverage	string	Minimum leverage
>> maxLeverage	string	Maximum leverage
>> leverageStep	string	The step to increase/reduce leverage
> priceFilter	Object	Price attributes
>> minPrice	string	Minimum order price
>> maxPrice	string	Maximum order price
>> tickSize	string	The step to increase/reduce order price
> lotSizeFilter	Object	Size attributes
>> minNotionalValue	string	Minimum notional value
>> maxOrderQty	string	Maximum quantity for Limit and PostOnly order
>> maxMktOrderQty	string	Maximum quantity for Market order
>> minOrderQty	string	Minimum order quantity
>> qtyStep	string	The step to increase/reduce order quantity
>> postOnlyMaxOrderQty	string	deprecated, please use maxOrderQty
> unifiedMarginTrade	boolean	Whether to support unified margin trade
> fundingInterval	integer	Funding interval (minute)
> settleCoin	string	Settle coin
> copyTrading	string	Copy trade symbol or not
> upperFundingRate	string	Upper limit of funding date
> lowerFundingRate	string	Lower limit of funding date
> displayName	string	The USDC futures & perpetual name displayed in the Web or App
> riskParameters	object	Risk parameters for limit order price. Note that the formula changed in Jan 2025
>> priceLimitRatioX	string	Ratio X
>> priceLimitRatioY	string	Ratio Y
> isPreListing	boolean	
Whether the contract is a pre-market contract
When the pre-market contract is converted to official contract, it will be false
> preListingInfo	object	
If isPreListing=false, preListingInfo=null
If isPreListing=true, preListingInfo is an object
>> curAuctionPhase	string	The current auction phase
>> phases	array<object>	Each phase time info
>>> phase	string	pre-market trading phase
>>> startTime	string	The start time of the phase, timestamp(ms)
>>> endTime	string	The end time of the phase, timestamp(ms)
>> auctionFeeInfo	object	Action fee info
>>> auctionFeeRate	string	The trading fee rate during auction phase
There is no trading fee until entering continues trading phase
>>> takerFeeRate	string	The taker fee rate during continues trading phase
>>> makerFeeRate	string	The maker fee rate during continues trading phase
RUN >>
Request Example
Linear
Option
Spot
HTTP
 
  
  
  
GET /v5/market/instruments-info?cate  ry=linear&symbol=BTCUSDT HTTP/1.1
Host: api-testnet.bybit.com

Response Example
Linear
Option
Spot
// official USDT Perpetual instrument structure
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "cate  ry": "linear",
        "list": [
            {
                "symbol": "BTCUSDT",
                "contractType": "LinearPerpetual",
                "status": "Trading",
                "baseCoin": "BTC",
                "quoteCoin": "USDT",
                "launchTime": "1585526400000",
                "deliveryTime": "0",
                "deliveryFeeRate": "",
                "priceScale": "2",
                "leverageFilter": {
                    "minLeverage": "1",
                    "maxLeverage": "100.00",
                    "leverageStep": "0.01"
                },
                "priceFilter": {
                    "minPrice": "0.10",
                    "maxPrice": "1999999.80",
                    "tickSize": "0.10"
                },
                "lotSizeFilter": {
                    "maxOrderQty": "1190.000",
                    "minOrderQty": "0.001",
                    "qtyStep": "0.001",
                    "postOnlyMaxOrderQty": "1190.000",
                    "maxMktOrderQty": "500.000",
                    "minNotionalValue": "5"
                },
                "unifiedMarginTrade": true,
                "fundingInterval": 480,
                "settleCoin": "USDT",
                "copyTrading": "both",
                "upperFundingRate": "0.00375",
                "lowerFundingRate": "-0.00375",
                "isPreListing": false,
                "preListingInfo": null,
                "riskParameters": {
                    "priceLimitRatioX": "0.01",
                    "priceLimitRatioY": "0.02"
                }
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1735809771618
}

// Pre-market Perpetual instrument structure
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "cate  ry": "linear",
        "list": [
            {
                "symbol": "BIOUSDT",
                "contractType": "LinearPerpetual",
                "status": "PreLaunch",
                "baseCoin": "BIO",
                "quoteCoin": "USDT",
                "launchTime": "1735032510000",
                "deliveryTime": "0",
                "deliveryFeeRate": "",
                "priceScale": "4",
                "leverageFilter": {
                    "minLeverage": "1",
                    "maxLeverage": "5.00",
                    "leverageStep": "0.01"
                },
                "priceFilter": {
                    "minPrice": "0.0001",
                    "maxPrice": "1999.9998",
                    "tickSize": "0.0001"
                },
                "lotSizeFilter": {
                    "maxOrderQty": "70000",
                    "minOrderQty": "1",
                    "qtyStep": "1",
                    "postOnlyMaxOrderQty": "70000",
                    "maxMktOrderQty": "14000",
                    "minNotionalValue": "5"
                },
                "unifiedMarginTrade": true,
                "fundingInterval": 480,
                "settleCoin": "USDT",
                "copyTrading": "none",
                "upperFundingRate": "0.05",
                "lowerFundingRate": "-0.05",
                "isPreListing": true,
                "preListingInfo": {
                    "curAuctionPhase": "ContinuousTrading",
                    "phases": [
                        {
                            "phase": "CallAuction",
                            "startTime": "1735113600000",
                            "endTime": "1735116600000"
                        },
                        {
                            "phase": "CallAuctionNoCancel",
                            "startTime": "1735116600000",
                            "endTime": "1735116900000"
                        },
                        {
                            "phase": "CrossMatching",
                            "startTime": "1735116900000",
                            "endTime": "1735117200000"
                        },
                        {
                            "phase": "ContinuousTrading",
                            "startTime": "1735117200000",
                            "endTime": ""
                        }
                    ],
                    "auctionFeeInfo": {
                        "auctionFeeRate": "0",
                        "takerFeeRate": "0.001",
                        "makerFeeRate": "0.0004"
                    }
                },
                "riskParameters": {
                    "priceLimitRatioX": "0.05",
                    "priceLimitRatioY": "0.1"
                }
            }
        ],
        "nextPageCursor": "first%3DBIOUSDT%26last%3DBIOUSDT"
    },
    "retExtInfo": {},
    "time": 1735810114435
}

Get Orderbook
Query for orderbook depth data.

Covers: Spot / USDT contract / USDC contract / Inverse contract / Option

Contract: 500-level of orderbook data
Spot: 200-level of orderbook data
Option: 25-level of orderbook data
info
The response is in the snapshot format.
Retail Price Improvement (RPI) orders will not be included in the response message and will not be visible over API.
HTTP Request
GET /v5/market/orderbook

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. spot, linear, inverse, option
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
limit	false	integer	Limit size for each bid and ask
spot: [1, 200]. Default: 1.
linear&inverse: [1, 500]. Default: 25.
option: [1, 25]. Default: 1.
Response Parameters
Parameter	Type	Comments
s	string	Symbol name
b	array	Bid, buyer. Sorted by price in descending order
> b[0]	string	Bid price
> b[1]	string	Bid size
a	array	Ask, seller. Sorted by price in ascending order
> a[0]	string	Ask price
> a[1]	string	Ask size
ts	integer	The timestamp (ms) that the system generates the data
u	integer	Update ID, is always in sequence
For contract, corresponds to u in the 500-level WebSocket orderbook stream
For spot, corresponds to u in the 200-level WebSocket orderbook stream
seq	integer	Cross sequence
You can use this field to compare different levels orderbook data, and for the smaller seq, then it means the data is generated earlier.
cts	integer	The timestamp from the matching engine when this orderbook data is produced. It can be correlated with T from public trade channel
RUN >>
Request Example
HTTP
 
  
  
  
GET /v5/market/orderbook?cate  ry=spot&symbol=BTCUSDT HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "s": "BTCUSDT",
        "a": [
            [
                "65557.7",
                "16.606555"
            ]
        ],
        "b": [
            [
                "65485.47",
                "47.081829"
            ]
        ],
        "ts": 1716863719031,
        "u": 230704,
        "seq": 1432604333,
        "cts": 1716863718905
    },
    "retExtInfo": {},
    "time": 1716863719382
}

Get Tickers
Query for the latest price snapshot, best bid/ask price, and trading volume in the last 24 hours.

Covers: Spot / USDT contract / USDC contract / Inverse contract / Option

info
If cate  ry=option, symbol or baseCoin must be passed.

HTTP Request
GET /v5/market/tickers

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. spot,linear,inverse,option
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
baseCoin	false	string	Base coin, uppercase only. Apply to option only
expDate	false	string	Expiry date. e.g., 25DEC22. Apply to option only
Response Parameters
Linear/Inverse
Option
Spot
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> lastPrice	string	Last price
> indexPrice	string	Index price
> markPrice	string	Mark price
> prevPrice24h	string	Market price 24 hours a  
> price24hPcnt	string	Percentage change of market price relative to 24h
> highPrice24h	string	The highest price in the last 24 hours
> lowPrice24h	string	The lowest price in the last 24 hours
> prevPrice1h	string	Market price an hour a  
> openInterest	string	Open interest size
> openInterestValue	string	Open interest value
> turnover24h	string	Turnover for 24h
> volume24h	string	Volume for 24h
> fundingRate	string	Funding rate
> nextFundingTime	string	Next funding time (ms)
> predictedDeliveryPrice	string	Predicated delivery price. It has a value 30 mins before delivery
> basisRate	string	Basis rate
> basis	string	Basis
> deliveryFeeRate	string	Delivery fee rate
> deliveryTime	string	Delivery timestamp (ms), applicable to expiry futures only
> ask1Size	string	Best ask size
> bid1Price	string	Best bid price
> ask1Price	string	Best ask price
> bid1Size	string	Best bid size
> preOpenPrice	string	Estimated pre-market contract open price
Meaningless once the market opens
> preQty	string	Estimated pre-market contract open qty
The value is meaningless once the market opens
> curPreListingPhase	string	The current pre-market contract phase
RUN >>
Request Example
GET /v5/market/tickers?cate  ry=inverse&symbol=BTCUSD HTTP/1.1
Host: api-testnet.bybit.com

Response Example
Inverse
Option
Spot
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "cate  ry": "inverse",
        "list": [
            {
                "symbol": "BTCUSD",
                "lastPrice": "16597.00",
                "indexPrice": "16598.54",
                "markPrice": "16596.00",
                "prevPrice24h": "16464.50",
                "price24hPcnt": "0.008047",
                "highPrice24h": "30912.50",
                "lowPrice24h": "15700.00",
                "prevPrice1h": "16595.50",
                "openInterest": "373504107",
                "openInterestValue": "22505.67",
                "turnover24h": "2352.94950046",
                "volume24h": "49337318",
                "fundingRate": "-0.001034",
                "nextFundingTime": "1672387200000",
                "predictedDeliveryPrice": "",
                "basisRate": "",
                "deliveryFeeRate": "",
                "deliveryTime": "0",
                "ask1Size": "1",
                "bid1Price": "16596.00",
                "ask1Price": "16597.50",
                "bid1Size": "1",
                "basis": ""
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672376496682
}

Get Funding Rate History
Query for historical funding rates. Each symbol has a different funding interval. For example, if the interval is 8 hours and the current time is UTC 12, then it returns the last funding rate, which settled at UTC 8.

To query the funding rate interval, please refer to the instruments-info endpoint.

Covers: USDT and USDC perpetual / Inverse perpetual

info
Passing only startTime returns an error.
Passing only endTime returns 200 records up till endTime.
Passing neither returns 200 records up till the current time.
HTTP Request
GET /v5/market/funding/history

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. linear,inverse
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
startTime	false	integer	The start timestamp (ms)
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 200]. Default: 200
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> fundingRate	string	Funding rate
> fundingRateTimestamp	string	Funding rate timestamp (ms)

Request Example

GET /v5/market/funding/history?cate  ry=linear&symbol=ETHPERP&limit=1 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "cate  ry": "linear",
        "list": [
            {
                "symbol": "ETHPERP",
                "fundingRate": "0.0001",
                "fundingRateTimestamp": "1672041600000"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672051897447
}

Get Recent Public Trades
Query recent public trading history in Bybit.

Covers: Spot / USDT contract / USDC contract / Inverse contract / Option

You can download archived historical trades from the website

HTTP Request
GET /v5/market/recent-trade

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. spot,linear,inverse,option
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
required for spot/linear/inverse
optional for option
baseCoin	false	string	Base coin, uppercase only
Apply to option only
If the field is not passed, return BTC data by default
optionType	false	string	Option type. Call or Put. Apply to option only
limit	false	integer	Limit for data size per page
spot: [1,60], default: 60
others: [1,1000], default: 500
Response Parameters
Parameter	Type	Comments
cate  ry	string	Products cate  ry
list	array	Object
> execId	string	Execution ID
> symbol	string	Symbol name
> price	string	Trade price
> size	string	Trade size
> side	string	Side of taker Buy, Sell
> time	string	Trade time (ms)
> isBlockTrade	boolean	Whether the trade is block trade
> isRPITrade	boolean	Whether the trade is RPI trade
> mP	string	Mark price, unique field for option
> iP	string	Index price, unique field for option
> mIv	string	Mark iv, unique field for option
> iv	string	iv, unique field for option
> seq	string	cross sequence
RUN >>
Request Example
HTTP
 
  
  
  
GET /v5/market/recent-trade?cate  ry=spot&symbol=BTCUSDT&limit=1 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "cate  ry": "spot",
        "list": [
            {
                "execId": "2100000000007764263",
                "symbol": "BTCUSDT",
                "price": "16618.49",
                "size": "0.00012",
                "side": "Buy",
                "time": "1672052955758",
                "isBlockTrade": false,
                "isRPITrade": true,
                "seq":"123456"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672053054358
}

Get Open Interest
Get the open interest of each symbol.

Covers: USDT contract / USDC contract / Inverse contract

info
The upper limit time you can query is the launch time of the symbol.
HTTP Request
GET /v5/market/open-interest

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. linear,inverse
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
intervalTime	true	string	Interval time. 5min,15min,30min,1h,4h,1d
startTime	false	integer	The start timestamp (ms)
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 200]. Default: 50
cursor	false	string	Cursor. Used to paginate
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
symbol	string	Symbol name
list	array	Object
> openInterest	string	Open interest. The value is the sum of both sides.
The unit of value, e.g., BTCUSD(inverse) is USD, BTCUSDT(linear) is BTC
> timestamp	string	The timestamp (ms)
nextPageCursor	string	Used to paginate
RUN >>
Request Example
HTTP
 
  
  
  
GET /v5/market/open-interest?cate  ry=inverse&symbol=BTCUSD&intervalTime=5min&startTime=1669571100000&endTime=1669571400000 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "symbol": "BTCUSD",
        "cate  ry": "inverse",
        "list": [
            {
                "openInterest": "461134384.00000000",
                "timestamp": "1669571400000"
            },
            {
                "openInterest": "461134292.00000000",
                "timestamp": "1669571100000"
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1672053548579
}

Get Historical Volatility
Query option historical volatility

Covers: Option

info
The data is hourly.
If both startTime and endTime are not specified, it will return the most recent 1 hours worth of data.
startTime and endTime are a pair of params. Either both are passed or they are not passed at all.
This endpoint can query the last 2 years worth of data, but make sure [endTime - startTime] <= 30 days.
HTTP Request
GET /v5/market/historical-volatility

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. option
baseCoin	false	string	Base coin, uppercase only. Default: return BTC data
quoteCoin	false	string	Quote coin, USD or USDT. Default: return quoteCoin=USD
period	false	integer	Period. If not specified, it will return data with a 7-day average by default
startTime	false	integer	The start timestamp (ms)
endTime	false	integer	The end timestamp (ms)
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> period	integer	Period
> value	string	Volatility
> time	string	Timestamp (ms)
RUN >>
Request Example
HTTP
 
  
  
GET /v5/market/historical-volatility?cate  ry=option&baseCoin=ETH&period=30 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "SUCCESS",
    "cate  ry": "option",
    "result": [
        {
            "period": 30,
            "value": "0.45024716",
            "time": "1672052400000"
        }
    ]
}

Get Insurance Pool
Query for Bybit insurance pool data (BTC/USDT/USDC etc)

info
The isolated insurance pool balance is updated every 1 minute, and shared insurance pool balance is updated every 24 hours
Please note that you may receive data from the previous minute. This is due to multiple backend containers starting at different times, which may cause a slight delay. You can always rely on the latest minute data for accuracy.
HTTP Request
GET /v5/market/insurance

Request Parameters
Parameter	Required	Type	Comments
coin	false	string	coin, uppercase only. Default: return all insurance coins
Response Parameters
Parameter	Type	Comments
updatedTime	string	Data updated time (ms)
list	array	Object
> coin	string	Coin
> symbols	string	
symbols with "BTCUSDT,ETHUSDT,SOLUSDT" mean these contracts are shared with one insurance pool
For an isolated insurance pool, it returns one contract
> balance	string	Balance
> value	string	USD value
RUN >>
Request Example
HTTP
 
  
  
  
GET /v5/market/insurance?coin=USDT HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "updatedTime": "1714003200000",
        "list": [
            {
                "coin": "USDT",
                "symbols": "MERLUSDT,10000000AIDOGEUSDT,ZEUSUSDT",
                "balance": "902178.57602476",
                "value": "901898.0963091522"
            },
            {
                "coin": "USDT",
                "symbols": "SOLUSDT,OMNIUSDT,AL  USDT",
                "balance": "14454.51626125",
                "value": "14449.515598975464"
            },
            {
                "coin": "USDT",
                "symbols": "XLMUSDT,WUSDT",
                "balance": "23.45018235",
                "value": "22.992864174376344"
            },
            {
                "coin": "USDT",
                "symbols": "AGIUSDT,WIFUSDT",
                "balance": "10002",
                "value": "9998.896846613574"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1714028451228
}


Get Risk Limit
Query for the risk limit.

Covers: USDT contract / USDC contract / Inverse contract

info
cate  ry=linear returns a data set of 15 symbols in each response. Please use the cursor param to get the next data set.

HTTP Request
GET /v5/market/risk-limit

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. linear,inverse
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the data set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> id	integer	Risk ID
> symbol	string	Symbol name
> riskLimitValue	string	Position limit
> maintenanceMargin	number	Maintain margin rate
> initialMargin	number	Initial margin rate
> isLowestRisk	integer	1: true, 0: false
> maxLeverage	string	Allowed max leverage
> mmDeduction	string	The maintenance margin deduction value when risk limit tier changed
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
  
  
GET /v5/market/risk-limit?cate  ry=inverse&symbol=BTCUSD HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "cate  ry": "inverse",
        "list": [
            {
                "id": 1,
                "symbol": "BTCUSD",
                "riskLimitValue": "150",
                "maintenanceMargin": "0.5",
                "initialMargin": "1",
                "isLowestRisk": 1,
                "maxLeverage": "100.00",
                "mmDeduction": ""
            },
        ....
        ]
    },
    "retExtInfo": {},
    "time": 1672054488010
}

Get Delivery Price
Get the delivery price.

Covers: USDT futures / USDC futures / Inverse futures / Option

info
Option: only returns those symbols which are DELIVERING (UTC 8 - UTC 12) when symbol is not specified.
HTTP Request
GET /v5/market/delivery-price

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. linear, inverse, option
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
baseCoin	false	string	Base coin, uppercase only. Default: BTC. Valid for option only
settleCoin	false	string	Settle coin, uppercase only. Default: USDC.
limit	false	integer	Limit for data size per page. [1, 200]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> deliveryPrice	string	Delivery price
> deliveryTime	string	Delivery timestamp (ms)
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
  
  
GET /v5/market/delivery-price?cate  ry=option&symbol=ETH-26DEC22-1400-C HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "cate  ry": "option",
        "nextPageCursor": "",
        "list": [
            {
                "symbol": "ETH-26DEC22-1400-C",
                "deliveryPrice": "1220.728594450",
                "deliveryTime": "1672041600000"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672055336993
}

Get Long Short Ratio
HTTP Request
GET /v5/market/account-ratio

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. linear(USDT Contract),inverse
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
period	true	string	Data recording period. 5min, 15min, 30min, 1h, 4h, 1d
startTime	false	string	The start timestamp (ms)
endTime	false	string	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 500]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> symbol	string	Symbol name
> buyRatio	string	The ratio of the number of long position
> sellRatio	string	The ratio of the number of short position
> timestamp	string	Timestamp (ms)
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
  
  
GET /v5/market/account-ratio?cate  ry=linear&symbol=BTCUSDT&period=1h&limit=2&startTime=1696089600000&endTime=1696262400000 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "symbol": "BTCUSDT",
                "buyRatio": "0.49",
                "sellRatio": "0.51",
                "timestamp": "1696262400000"
            },
            {
                "symbol": "BTCUSDT",
                "buyRatio": "0.4927",
                "sellRatio": "0.5073",
                "timestamp": "1696258800000"
            }
        ],
        "nextPageCursor": "lastid%3D0%26lasttime%3D1696258800"
    },
    "retExtInfo": {},
    "time": 1731567491688
}

Get Order Price Limit
For derivative trading order price limit, refer to announcement
For spot trading order price limit, refer to announcement

HTTP Request
GET /v5/market/price-limit

Request Parameters
Parameter	Required	Type	Comments
cate  ry	false	string	Product type. spot,linear,inverse
When cate  ry is not passed, use linear by default
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
Response Parameters
Parameter	Type	Comments
symbol	string	Symbol name
buyLmt	string	Highest Bid Price
sellLmt	string	Lowest Ask Price
ts	string	timestamp in milliseconds
Request Example
HTTP
 
  
  
  
GET /v5/market/price-limit?cate  ry=linear&symbol=BTCUSDT HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "symbol": "BTCUSDT",
        "buyLmt": "105878.10",
        "sellLmt": "103781.60",
        "ts": "1750302284491"
    },
    "retExtInfo": {},
    "time": 1750302285376
}

Place Order
This endpoint supports to create the order for Spot, Margin trading, USDT perpetual, USDT futures, USDC perpetual, USDC futures, Inverse Futures and Options.

info
Supported order type (orderType):
Limit order: orderType=Limit, it is necessary to specify order qty and price.

Market order: orderType=Market, execute at the best price in the Bybit market until the transaction is completed. When selecting a market order, the "price" can be empty. In the futures trading system, in order to protect traders against the serious slippage of the Market order, Bybit trading engine will convert the market order into an IOC limit order for matching. If there are no orderbook entries within price slippage limit, the order will not be executed. If there is insufficient liquidity, the order will be cancelled. The slippage threshold refers to the percentage that the order price deviates from the mark price. You can learn more here: Adjustments to Bybit's Derivative Trading Price Limit Mechanism
Supported timeInForce strategy:
GTC
IOC
FOK
PostOnly: If the order would be filled immediately when submitted, it will be cancelled. The purpose of this is to protect your order during the submission process. If the matching system cannot entrust the order to the order book due to price changes on the market, it will be cancelled.
RPI: Retail Price Improvement order. Assigned market maker can place this kind of order, and it is a post only order, only match with the order from Web or APP.

How to create a conditional order:
When submitting an order, if triggerPrice is set, the order will be automatically converted into a conditional order. In addition, the conditional order does not occupy the margin. If the margin is insufficient after the conditional order is triggered, the order will be cancelled.

Take profit / Stop loss: You can set TP/SL while placing orders. Besides, you could modify the position's TP/SL.

Order quantity: The quantity of perpetual contracts you are   ing to buy/sell. For the order quantity, Bybit only supports positive number at present.

Order price: Place a limit order, this parameter is required. If you have position, the price should be higher than the liquidation price. For the minimum unit of the price change, please refer to the priceFilter > tickSize field in the instruments-info endpoint.

orderLinkId: You can customize the active order ID. We can link this ID to the order ID in the system. Once the active order is successfully created, we will send the unique order ID in the system to you. Then, you can use this order ID to cancel active orders, and if both orderId and orderLinkId are entered in the parameter input, Bybit will prioritize the orderId to process the corresponding order. Meanwhile, your customized order ID should be no longer than 36 characters and should be unique.

Open orders up limit:
Perps & Futures:
a) Each account can hold a maximum of 500 active orders simultaneously per symbol.
b) conditional orders: each account can hold a maximum of 10 active orders simultaneously per symbol.
Spot: 500 orders in total, including a maximum of 30 open TP/SL orders, a maximum of 30 open conditional orders for each symbol per account
Option: a maximum of 50 open orders per account

Rate limit:
Please refer to rate limit table. If you need to raise the rate limit, please contact your client manager or submit an application via here

Risk control limit notice:
Bybit will monitor on your API requests. When the total number of orders of a single user (aggregated the number of orders across main account and subaccounts) within a day (UTC 0 - UTC 24) exceeds a certain upper limit, the platform will reserve the right to remind, warn, and impose necessary restrictions. Customers who use API default to acceptance of these terms and have the obligation to cooperate with adjustments.

Spot Stop Order
Spot supports TP/SL order, Conditional order, however, the system logic is different between classic account and Unified account
classic account: When the stop order is created, you will get an order ID. After it is triggered, you will get a new order ID
Unified account: When the stop order is created, you will get an order ID. After it is triggered, the order ID will not be changed

HTTP Request
POST /v5/order/create

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse, spot, option
classic account: linear, inverse, spot
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
isLeverage	false	integer	Whether to borrow. Unified account Spot trading only.
0(default): false, spot trading
1: true, margin trading, make sure you turn on margin trading, and set the relevant currency as collateral
side	true	string	Buy, Sell
orderType	true	string	Market, Limit
qty	true	string	Order quantity
UTA account
Spot: Market Buy order by value by default, you can set marketUnit field to choose order by value or qty for market orders
Perps, Futures & Option: always order by qty
classic account
Spot: Market Buy order by value by default
Perps, Futures: always order by qty
Perps & Futures: if you pass qty="0" and specify reduceOnly=true&closeOnTrigger=true, you can close the position up to maxMktOrderQty or maxOrderQty shown on Get Instruments Info of current symbol
marketUnit	false	string	Select the unit for qty when create Spot market orders for UTA account
baseCoin: for example, buy BTCUSDT, then "qty" unit is BTC
quoteCoin: for example, sell BTCUSDT, then "qty" unit is USDT
slippageToleranceType	false	string	Slippage tolerance Type for market order, TickSize, Percent
Support linear, inverse, spot trading, but take profit, stoploss, conditional orders are not supported
TickSize:
the highest price of Buy order = ask1 + slippageTolerance x tickSize;
the lowest price of Sell order = bid1 - slippageTolerance x tickSize
Percent:
the highest price of Buy order = ask1 x (1 + slippageTolerance x 0.01);
the lowest price of Sell order = bid1 x (1 - slippageTolerance x 0.01)
slippageTolerance	false	string	Slippage tolerance value
TickSize: range is [5, 2000], integer only
Percent: range is [0.05, 1], up to 2 decimals
price	false	string	Order price
Market order will ignore this field
Please check the min price and price precision from instrument info endpoint
If you have position, price needs to be better than liquidation price
triggerDirection	false	integer	Conditional order param. Used to identify the expected direction of the conditional order.
1: triggered when market price rises to triggerPrice
2: triggered when market price falls to triggerPrice
Valid for linear & inverse
orderFilter	false	string	If it is not passed, Order by default.
Order
tpslOrder: Spot TP/SL order, the assets are occupied even before the order is triggered
StopOrder: Spot conditional order, the assets will not be occupied until the price of the underlying asset reaches the trigger price, and the required assets will be occupied after the Conditional order is triggered
Valid for spot only
triggerPrice	false	string	
For Perps & Futures, it is the conditional order trigger price. If you expect the price to rise to trigger your conditional order, make sure:
triggerPrice > market price
Else, triggerPrice < market price
For spot, it is the TP/SL and Conditional order trigger price
triggerBy	false	string	Trigger price type, Conditional order param for Perps & Futures.
LastPrice
IndexPrice
MarkPrice
Valid for linear & inverse
orderIv	false	string	Implied volatility. option only. Pass the real value, e.g for 10%, 0.1 should be passed. orderIv has a higher priority when price is passed as well
timeInForce	false	string	Time in force
Market order will always use IOC
If not passed, GTC is used by default
positionIdx	false	integer	Used to identify positions in different position modes. Under hedge-mode, this param is required
0: one-way mode
1: hedge-mode Buy side
2: hedge-mode Sell side
orderLinkId	false	string	User customised order ID. A max of 36 characters. Combinations of numbers, letters (upper and lower cases), dashes, and underscores are supported.
Futures & Perps: orderLinkId rules:
optional param
always unique
option orderLinkId rules:
required param
always unique
takeProfit	false	string	Take profit price
UTA: Spot Limit order supports take profit, stop loss or limit take profit, limit stop loss when creating an order
stopLoss	false	string	Stop loss price
UTA: Spot Limit order supports take profit, stop loss or limit take profit, limit stop loss when creating an order
tpTriggerBy	false	string	The price type to trigger take profit. MarkPrice, IndexPrice, default: LastPrice. Valid for linear & inverse
slTriggerBy	false	string	The price type to trigger stop loss. MarkPrice, IndexPrice, default: LastPrice. Valid for linear & inverse
reduceOnly	false	boolean	What is a reduce-only order? true means your position can only reduce in size if this order is triggered.
You must specify it as true when you are about to close/reduce the position
When reduceOnly is true, take profit/stop loss cannot be set
Valid for linear, inverse & option
closeOnTrigger	false	boolean	What is a close on trigger order? For a closing order. It can only reduce your position, not increase it. If the account has insufficient available balance when the closing order is triggered, then other active orders of similar contracts will be cancelled or reduced. It can be used to ensure your stop loss reduces your position regardless of current available margin.
Valid for linear & inverse
smpType	false	string	Smp execution type. What is SMP?
mmp	false	boolean	Market maker protection. option only. true means set the order as a market maker protection order. What is mmp?
tpslMode	false	string	TP/SL mode
Full: entire position for TP/SL. Then, tpOrderType or slOrderType must be Market
Partial: partial position tp/sl (as there is no size option, so it will create tp/sl orders with the qty you actually fill). Limit TP/SL order are supported. Note: When create limit tp/sl, tpslMode is required and it must be Partial
Valid for linear & inverse
tpLimitPrice	false	string	The limit order price when take profit price is triggered
linear & inverse: only works when tpslMode=Partial and tpOrderType=Limit
Spot(UTA): it is required when the order has takeProfit and "tpOrderType"=Limit
slLimitPrice	false	string	The limit order price when stop loss price is triggered
linear & inverse: only works when tpslMode=Partial and slOrderType=Limit
Spot(UTA): it is required when the order has stopLoss and "slOrderType"=Limit
tpOrderType	false	string	The order type when take profit is triggered
linear & inverse: Market(default), Limit. For tpslMode=Full, it only supports tpOrderType=Market
Spot(UTA):
Market: when you set "takeProfit",
Limit: when you set "takeProfit" and "tpLimitPrice"
slOrderType	false	string	The order type when stop loss is triggered
linear & inverse: Market(default), Limit. For tpslMode=Full, it only supports slOrderType=Market
Spot(UTA):
Market: when you set "stopLoss",
Limit: when you set "stopLoss" and "slLimitPrice"
Response Parameters
Parameter	Type	Comments
orderId	string	Order ID
orderLinkId	string	User customised order ID
info
The acknowledgement of an place order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

RUN >>
Request Example
HTTP
 
  
  
.Net
  
POST /v5/order/create HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672211928338
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

// Spot Limit order with market tp sl
{"cate  ry": "spot","symbol": "BTCUSDT","side": "Buy","orderType": "Limit","qty": "0.01","price": "28000","timeInForce": "PostOnly","takeProfit": "35000","stopLoss": "27000","tpOrderType": "Market","slOrderType": "Market"}

// Spot Limit order with limit tp sl
{"cate  ry": "spot","symbol": "BTCUSDT","side": "Buy","orderType": "Limit","qty": "0.01","price": "28000","timeInForce": "PostOnly","takeProfit": "35000","stopLoss": "27000","tpLimitPrice": "36000","slLimitPrice": "27500","tpOrderType": "Limit","slOrderType": "Limit"}

// Spot PostOnly normal order
{"cate  ry":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"0.1","price":"15600","timeInForce":"PostOnly","orderLinkId":"spot-test-01","isLeverage":0,"orderFilter":"Order"}

// Spot TP/SL order
{"cate  ry":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"0.1","price":"15600","triggerPrice": "15000", "timeInForce":"Limit","orderLinkId":"spot-test-02","isLeverage":0,"orderFilter":"tpslOrder"}

// Spot margin normal order (UTA)
{"cate  ry":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"0.1","price":"15600","timeInForce":"GTC","orderLinkId":"spot-test-limit","isLeverage":1,"orderFilter":"Order"}

// Spot Market Buy order, qty is quote currency
{"cate  ry":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Market","qty":"200","timeInForce":"IOC","orderLinkId":"spot-test-04","isLeverage":0,"orderFilter":"Order"}


// USDT Perp open long position (one-way mode)
{"cate  ry":"linear","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"1","price":"25000","timeInForce":"GTC","positionIdx":0,"orderLinkId":"usdt-test-01","reduceOnly":false,"takeProfit":"28000","stopLoss":"20000","tpslMode":"Partial","tpOrderType":"Limit","slOrderType":"Limit","tpLimitPrice":"27500","slLimitPrice":"20500"}

// USDT Perp close long position (one-way mode)
{"cate  ry": "linear", "symbol": "BTCUSDT", "side": "Sell", "orderType": "Limit", "qty": "1", "price": "30000", "timeInForce": "GTC", "positionIdx": 0, "orderLinkId": "usdt-test-02", "reduceOnly": true}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderId": "1321003749386327552",
        "orderLinkId": "spot-test-postonly"
    },
    "retExtInfo": {},
    "time": 1672211918471
}

Amend Order
info
You can only modify unfilled or partially filled orders.

HTTP Request
POST /v5/order/amend

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse, spot, option
classic account: linear, inverse, spot
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
orderId	false	string	Order ID. Either orderId or orderLinkId is required
orderLinkId	false	string	User customised order ID. Either orderId or orderLinkId is required
orderIv	false	string	Implied volatility. option only. Pass the real value, e.g for 10%, 0.1 should be passed
triggerPrice	false	string	
For Perps & Futures, it is the conditional order trigger price. If you expect the price to rise to trigger your conditional order, make sure:
triggerPrice > market price
Else, triggerPrice < market price
For spot, it is the TP/SL and Conditional order trigger price
qty	false	string	Order quantity after modification. Do not pass it if not modify the qty
price	false	string	Order price after modification. Do not pass it if not modify the price
tpslMode	false	string	TP/SL mode
Full: entire position for TP/SL. Then, tpOrderType or slOrderType must be Market
Partial: partial position tp/sl. Limit TP/SL order are supported. Note: When create limit tp/sl, tpslMode is required and it must be Partial
Valid for linear & inverse
takeProfit	false	string	Take profit price after modification. If pass "0", it means cancel the existing take profit of the order. Do not pass it if you do not want to modify the take profit. valid for spot(UTA), linear, inverse
stopLoss	false	string	Stop loss price after modification. If pass "0", it means cancel the existing stop loss of the order. Do not pass it if you do not want to modify the stop loss. valid for spot(UTA), linear, inverse
tpTriggerBy	false	string	The price type to trigger take profit. When set a take profit, this param is required if no initial value for the order
slTriggerBy	false	string	The price type to trigger stop loss. When set a take profit, this param is required if no initial value for the order
triggerBy	false	string	Trigger price type
tpLimitPrice	false	string	Limit order price when take profit is triggered. Only working when original order sets partial limit tp/sl. valid for spot(UTA), linear, inverse
slLimitPrice	false	string	Limit order price when stop loss is triggered. Only working when original order sets partial limit tp/sl. valid for spot(UTA), linear, inverse
info
The acknowledgement of an amend order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

RUN >>
Response Parameters
Parameter	Type	Comments
orderId	string	Order ID
orderLinkId	string	User customised order ID
Request Example
HTTP
 
  
.Net
  
POST /v5/order/amend HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672217108106
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "cate  ry": "linear",
    "symbol": "ETHPERP",
    "orderLinkId": "linear-004",
    "triggerPrice": "1145",
    "qty": "0.15",
    "price": "1050",
    "takeProfit": "0",
    "stopLoss": "0"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderId": "c6f055d9-7f21-4079-913d-e6523a9cfffa",
        "orderLinkId": "linear-004"
    },
    "retExtInfo": {},
    "time": 1672217093461
}

Cancel Order
important
You must specify orderId or orderLinkId to cancel the order.
If orderId and orderLinkId do not match, the system will process orderId first.
You can only cancel unfilled or partially filled orders.
HTTP Request
POST /v5/order/cancel

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse, spot, option
classic account: linear, inverse, spot
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
orderId	false	string	Order ID. Either orderId or orderLinkId is required
orderLinkId	false	string	User customised order ID. Either orderId or orderLinkId is required
orderFilter	false	string	Spot trading only
Order
tpslOrder
StopOrder
If not passed, Order by default
Response Parameters
Parameter	Type	Comments
orderId	string	Order ID
orderLinkId	string	User customised order ID
info
The acknowledgement of an cancel order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

RUN >>
Request Example
HTTP
 
  
.Net
  
POST /v5/order/cancel HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672217376681
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
  "cate  ry": "linear",
  "symbol": "BTCPERP",
  "orderLinkId": null,
  "orderId":"c6f055d9-7f21-4079-913d-e6523a9cfffa"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderId": "c6f055d9-7f21-4079-913d-e6523a9cfffa",
        "orderLinkId": "linear-004"
    },
    "retExtInfo": {},
    "time": 1672217377164
}

Get Open & Closed Orders
Primarily query unfilled or partially filled orders in real-time, but also supports querying recent 500 closed status (Cancelled, Filled) orders. Please see the usage of request param openOnly.
And to query older order records, please use the order history interface.

tip
UTA2.0 can query filled, cancelled, and rejected orders to the most recent 500 orders for spot, linear, inverse and option cate  ries
UTA1.0 can query filled, cancelled, and rejected orders to the most recent 500 orders for spot, linear, and option cate  ries. The inverse cate  ry is not subject to this limitation.
You can query by symbol, baseCoin, orderId and orderLinkId, and if you pass multiple params, the system will process them according to this priority: orderId > orderLinkId > symbol > baseCoin.
The records are sorted by the createdTime from newest to oldest.
info
classic account spot can return open orders only
After a server release or restart, filled, cancelled, and rejected orders of Unified account should only be queried through order history.
HTTP Request
GET /v5/order/realtime

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse, spot, option
classic account: linear, inverse, spot
symbol	false	string	Symbol name, like BTCUSDT, uppercase only. For linear, either symbol, baseCoin, settleCoin is required
baseCoin	false	string	Base coin, uppercase only
Supports linear, inverse & option
option: it returns all option open orders by default
settleCoin	false	string	Settle coin, uppercase only
linear: either symbol, baseCoin or settleCoin is required
spot: not supported
option: USDT or USDC
orderId	false	string	Order ID
orderLinkId	false	string	User customised order ID
openOnly	false	integer	
0(default): UTA2.0, UTA1.0, classic account query open status orders (e.g., New, PartiallyFilled) only
1: UTA2.0, UTA1.0(except inverse)
2: UTA1.0(inverse), classic account
Query a maximum of recent 500 closed status records are kept under each account each cate  ry (e.g., Cancelled, Rejected, Filled orders).
If the Bybit service is restarted due to an update, this part of the data will be cleared and accumulated again, but the order records will still be queried in order history
openOnly param will be ignored when query by orderId or orderLinkId
Classic spot: not supported
orderFilter	false	string	Order: active order, StopOrder: conditional order for Futures and Spot, tpslOrder: spot TP/SL order, OcoOrder: Spot oco order, BidirectionalTpslOrder: Spot bidirectional TPSL order
classic account spot: return Order active order by default
Others: all kinds of orders by default
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
nextPageCursor	string	Refer to the cursor request parameter
list	array	Object
> orderId	string	Order ID
> orderLinkId	string	User customised order ID
> blockTradeId	string	Paradigm block trade ID
> symbol	string	Symbol name
> price	string	Order price
> qty	string	Order qty
> side	string	Side. Buy,Sell
> isLeverage	string	Whether to borrow. Unified spot only. 0: false, 1: true. Classic spot is not supported, always 0
> positionIdx	integer	Position index. Used to identify positions in different position modes.
> orderStatus	string	Order status
> createType	string	Order create type
Only for cate  ry=linear or inverse
Spot, Option do not have this key
> cancelType	string	Cancel type
> rejectReason	string	Reject reason. Classic spot is not supported
> avgPrice	string	Average filled price
UTA: returns "" for those orders without avg price
classic account: returns "0" for those orders without avg price, and also for those orders have partilly filled but cancelled at the end
> leavesQty	string	The remaining qty not executed. Classic spot is not supported
> leavesValue	string	The estimated value not executed. Classic spot is not supported
> cumExecQty	string	Cumulative executed order qty
> cumExecValue	string	Cumulative executed order value. Classic spot is not supported
> cumExecFee	string	Cumulative executed trading fee. Classic spot is not supported
> timeInForce	string	Time in force
> orderType	string	Order type. Market,Limit. For TP/SL order, it means the order type after triggered
> stopOrderType	string	Stop order type
> orderIv	string	Implied volatility
> marketUnit	string	The unit for qty when create Spot market orders for UTA account. baseCoin, quoteCoin
> triggerPrice	string	Trigger price. If stopOrderType=TrailingStop, it is activate price. Otherwise, it is trigger price
> takeProfit	string	Take profit price
> stopLoss	string	Stop loss price
> tpslMode	string	TP/SL mode, Full: entire position for TP/SL. Partial: partial position tp/sl. Spot does not have this field, and Option returns always ""
> ocoTriggerBy	string	The trigger type of Spot OCO order.OcoTriggerByUnknown, OcoTriggerByTp, OcoTriggerByBySl. Classic spot is not supported
> tpLimitPrice	string	The limit order price when take profit price is triggered
> slLimitPrice	string	The limit order price when stop loss price is triggered
> tpTriggerBy	string	The price type to trigger take profit
> slTriggerBy	string	The price type to trigger stop loss
> triggerDirection	integer	Trigger direction. 1: rise, 2: fall
> triggerBy	string	The price type of trigger price
> lastPriceOnCreated	string	Last price when place the order, Spot is not applicable
> basePrice	string	Last price when place the order, Spot has this field only
> reduceOnly	boolean	Reduce only. true means reduce position size
> closeOnTrigger	boolean	Close on trigger. What is a close on trigger order?
> placeType	string	Place type, option used. iv, price
> smpType	string	SMP execution type
> smpGroup	integer	Smp group ID. If the UID has no group, it is 0 by default
> smpOrderId	string	The counterparty's orderID which triggers this SMP execution
> createdTime	string	Order created timestamp (ms)
> updatedTime	string	Order updated timestamp (ms)
RUN >>
Request Example
HTTP
 
  
  
GET /v5/order/realtime?symbol=ETHUSDT&cate  ry=linear&openOnly=0&limit=1  HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672219525810
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "orderId": "fd4300ae-7847-404e-b947-b46980a4d140",
                "orderLinkId": "test-000005",
                "blockTradeId": "",
                "symbol": "ETHUSDT",
                "price": "1600.00",
                "qty": "0.10",
                "side": "Buy",
                "isLeverage": "",
                "positionIdx": 1,
                "orderStatus": "New",
                "cancelType": "UNKNOWN",
                "rejectReason": "EC_NoError",
                "avgPrice": "0",
                "leavesQty": "0.10",
                "leavesValue": "160",
                "cumExecQty": "0.00",
                "cumExecValue": "0",
                "cumExecFee": "0",
                "timeInForce": "GTC",
                "orderType": "Limit",
                "stopOrderType": "UNKNOWN",
                "orderIv": "",
                "triggerPrice": "0.00",
                "takeProfit": "2500.00",
                "stopLoss": "1500.00",
                "tpTriggerBy": "LastPrice",
                "slTriggerBy": "LastPrice",
                "triggerDirection": 0,
                "triggerBy": "UNKNOWN",
                "lastPriceOnCreated": "",
                "reduceOnly": false,
                "closeOnTrigger": false,
                "smpType": "None",
                "smpGroup": 0,
                "smpOrderId": "",
                "tpslMode": "Full",
                "tpLimitPrice": "",
                "slLimitPrice": "",
                "placeType": "",
                "createdTime": "1684738540559",
                "updatedTime": "1684738540561"
            }
        ],
        "nextPageCursor": "page_args%3Dfd4300ae-7847-404e-b947-b46980a4d140%26symbol%3D6%26",
        "cate  ry": "linear"
    },
    "retExtInfo": {},
    "time": 1684765770483
}

Cancel All Orders
Cancel all open orders

info
Support cancel orders by symbol/baseCoin/settleCoin. If you pass multiple of these params, the system will process one of param, which priority is symbol > baseCoin > settleCoin.
NOTE: cate  ry=option, you can cancel all option open orders without passing any of those three params. However, for "linear" and "inverse", you must specify one of those three params.
NOTE: cate  ry=spot, you can cancel all spot open orders (normal order by default) without passing other params.
info
Spot: classic account - cancel up to 500 orders; Unified account - no limit
Futures: classic account - cancel up to 500 orders; Unified account - cancel up to 500 orders (System picks up 500 orders randomly to cancel when you have over 500 orders)
Options: Unified account - no limit

HTTP Request
POST /v5/order/cancel-all

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse, spot, option
classic account: linear, inverse, spot
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
linear&inverse: Required if not passing baseCoin or settleCoin
baseCoin	false	string	Base coin, uppercase only
linear & inverse(classic account): If cancel all by baseCoin, it will cancel all linear & inverse orders. Required if not passing symbol or settleCoin
linear & inverse(Unified account): If cancel all by baseCoin, it will cancel all corresponding cate  ry orders. Required if not passing symbol or settleCoin
spot(classic account): invalid
settleCoin	false	string	Settle coin, uppercase only
linear & inverse: Required if not passing symbol or baseCoin
option: USDT or USDC
Not support spot
orderFilter	false	string	
cate  ry=spot, you can pass Order, tpslOrder, StopOrder, OcoOrder, BidirectionalTpslOrder
If not passed, Order by default
cate  ry=linear or inverse, you can pass Order, StopOrder,OpenOrder
If not passed, all kinds of orders will be cancelled, like active order, conditional order, TP/SL order and trailing stop order
cate  ry=option, you can pass Order
No matter it is passed or not, always cancel all orders
stopOrderType	false	string	Stop order type Stop
Only used for cate  ry=linear or inverse and orderFilter=StopOrder,you can cancel conditional orders except TP/SL order and Trailing stop orders with this param
info
The acknowledgement of create/amend/cancel order requests indicates that the request was sucessfully accepted. The request is asynchronous so please use the websocket to confirm the order status.

RUN >>
Response Parameters
Parameter	Type	Comments
list	array	Object
> orderId	string	Order ID
> orderLinkId	string	User customised order ID
success	string	"1": success, "0": fail
UTA1.0(inverse), classic(linear, inverse) do not return this field
Request Example
HTTP
 
  
.Net
  
POST /v5/order/cancel-all HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672219779140
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
  "cate  ry": "linear",
  "symbol": null,
  "settleCoin": "USDT"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "orderId": "1616024329462743808",
                "orderLinkId": "1616024329462743809"
            },
            {
                "orderId": "1616024287544869632",
                "orderLinkId": "1616024287544869633"
            }
        ],
        "success": "1"
    },
    "retExtInfo": {},
    "time": 1707381118116
}

Get Order History
Query order history. As order creation/cancellation is asynchronous, the data returned from this endpoint may delay. If you want to get real-time order information, you could query this endpoint or rely on the websocket stream (recommended).

rule
The orders in the last 7 days:
UTA2.0, UTA1.0(except inverse) support querying all closed status except "Cancelled", "Rejected", "Deactivated" status.
UTA1.0(inverse) and classic account support querying all status (open and close status)
The orders in the last 24 hours:
UTA2.0, UTA1.0(except inverse) for the orders with "Cancelled" (fully cancelled order), "Rejected", "Deactivated" can be query
The orders beyond 7 days:
All account supports querying orders which have fills only, i.e., fully filled, partial filled but cancelled orders
UTA2.0, UTA1.0(except inverse) support querying the past 2 years data.
info
Classic Spot can get closed order status only, and Cancelled, Rejected, Deactivated orders save up to 7 days
HTTP Request
GET /v5/order/history

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse, spot, option
classic account: linear, inverse, spot
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
baseCoin	false	string	Base coin, uppercase only
UTA1.0(inverse), classic account do not support this param
settleCoin	false	string	Settle coin, uppercase only
UTA1.0(inverse), classic account do not support this param
orderId	false	string	Order ID
orderLinkId	false	string	User customised order ID
orderFilter	false	string	Order: active order
StopOrder: conditional order for Futures and Spot
tpslOrder: spot TP/SL order
OcoOrder: spot OCO orders
BidirectionalTpslOrder: Spot bidirectional TPSL order
classic account spot: return Order active order by default
Others: all kinds of orders by default
orderStatus	false	string	
Classic spot: not supported
UTA2.0, UTA1.0(except inverse): return all closed status orders if not passed
UTA1.0(inverse), classic account(linear, inverse): return all status orders if not passed
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 7 days by default
Only startTime is passed, return range between startTime and startTime+7 days
Only endTime is passed, return range between endTime-7 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> orderId	string	Order ID
> orderLinkId	string	User customised order ID
> blockTradeId	string	Block trade ID
> symbol	string	Symbol name
> price	string	Order price
> qty	string	Order qty
> side	string	Side. Buy,Sell
> isLeverage	string	Whether to borrow. Unified spot only. 0: false, 1: true. . Classic spot is not supported, always 0
> positionIdx	integer	Position index. Used to identify positions in different position modes
> orderStatus	string	Order status
> createType	string	Order create type
Only for cate  ry=linear or inverse
Spot, Option do not have this key
> cancelType	string	Cancel type
> rejectReason	string	Reject reason. Classic spot is not supported
> avgPrice	string	Average filled price
UTA: returns "" for those orders without avg price
classic account: returns "0" for those orders without avg price, and also for those orders have partilly filled but cancelled at the end
> leavesQty	string	The remaining qty not executed. Classic spot is not supported
> leavesValue	string	The estimated value not executed. Classic spot is not supported
> cumExecQty	string	Cumulative executed order qty
> cumExecValue	string	Cumulative executed order value. Classic spot is not supported
> cumExecFee	string	Cumulative executed trading fee. Classic spot is not supported
> timeInForce	string	Time in force
> orderType	string	Order type. Market,Limit. For TP/SL order, it means the order type after triggered
Block trade Roll Back, Block trade-Limit: Unique enum values for Unified account block trades
> stopOrderType	string	Stop order type
> orderIv	string	Implied volatility
> marketUnit	string	The unit for qty when create Spot market orders for UTA account. baseCoin, quoteCoin
> slippageToleranceType	string	Spot and Futures market order slippage tolerance type TickSize, Percent, UNKNOWN(default)
> slippageTolerance	string	Slippage tolerance value
> triggerPrice	string	Trigger price. If stopOrderType=TrailingStop, it is activate price. Otherwise, it is trigger price
> takeProfit	string	Take profit price
> stopLoss	string	Stop loss price
> tpslMode	string	TP/SL mode, Full: entire position for TP/SL. Partial: partial position tp/sl. Spot does not have this field, and Option returns always ""
> ocoTriggerBy	string	The trigger type of Spot OCO order.OcoTriggerByUnknown, OcoTriggerByTp, OcoTriggerBySl. Classic spot is not supported
> tpLimitPrice	string	The limit order price when take profit price is triggered
> slLimitPrice	string	The limit order price when stop loss price is triggered
> tpTriggerBy	string	The price type to trigger take profit
> slTriggerBy	string	The price type to trigger stop loss
> triggerDirection	integer	Trigger direction. 1: rise, 2: fall
> triggerBy	string	The price type of trigger price
> lastPriceOnCreated	string	Last price when place the order, Spot is not applicable
> basePrice	string	Last price when place the order, Spot has this field only
> reduceOnly	boolean	Reduce only. true means reduce position size
> closeOnTrigger	boolean	Close on trigger. What is a close on trigger order?
> placeType	string	Place type, option used. iv, price
> smpType	string	SMP execution type
> smpGroup	integer	Smp group ID. If the UID has no group, it is 0 by default
> smpOrderId	string	The counterparty's orderID which triggers this SMP execution
> createdTime	string	Order created timestamp (ms)
> updatedTime	string	Order updated timestamp (ms)
> extraFees	string	Trading fee rate information. Currently, this data is returned only for spot orders placed on the Indonesian site or spot fiat currency orders placed on the EU site. In other cases, an empty string is returned. Enum: feeType, subFeeType
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
  
GET /v5/order/history?cate  ry=linear&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672221263407
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "orderId": "14bad3a1-6454-43d8-bcf2-5345896cf74d",
                "orderLinkId": "YLxaWKMiHU",
                "blockTradeId": "",
                "symbol": "BTCUSDT",
                "price": "26864.40",
                "qty": "0.003",
                "side": "Buy",
                "isLeverage": "",
                "positionIdx": 1,
                "orderStatus": "Cancelled",
                "cancelType": "UNKNOWN",
                "rejectReason": "EC_PostOnlyWillTakeLiquidity",
                "avgPrice": "0",
                "leavesQty": "0.000",
                "leavesValue": "0",
                "cumExecQty": "0.000",
                "cumExecValue": "0",
                "cumExecFee": "0",
                "timeInForce": "PostOnly",
                "orderType": "Limit",
                "stopOrderType": "UNKNOWN",
                "orderIv": "",
                "triggerPrice": "0.00",
                "takeProfit": "0.00",
                "stopLoss": "0.00",
                "tpTriggerBy": "UNKNOWN",
                "slTriggerBy": "UNKNOWN",
                "triggerDirection": 0,
                "triggerBy": "UNKNOWN",
                "lastPriceOnCreated": "0.00",
                "reduceOnly": false,
                "closeOnTrigger": false,
                "smpType": "None",
                "smpGroup": 0,
                "smpOrderId": "",
                "tpslMode": "",
                "tpLimitPrice": "",
                "slLimitPrice": "",
                "placeType": "",
                "slippageToleranceType": "UNKNOWN",
                "slippageTolerance": "",
                "createdTime": "1684476068369",
                "updatedTime": "1684476068372",
                "extraFees": ""
            }
        ],
        "nextPageCursor": "page_token%3D39380%26",
        "cate  ry": "linear"
    },
    "retExtInfo": {},
    "time": 1684766282976
}

Get Trade History
Query users' execution records, sorted by execTime in descending order. However, for Classic spot, they are sorted by execId in descending order.

tip
Response items will have sorting issues When 'execTime' is the same, it is recommended to sort according to execId+OrderId+leavesQty. This issue is currently being optimized and will be released. If you want to receive real-time execution information, Use the websocket stream (recommended).
You may have multiple executions in a single order.
You can query by symbol, baseCoin, orderId and orderLinkId, and if you pass multiple params, the system will process them according to this priority: orderId > orderLinkId > symbol > baseCoin. orderId and orderLinkId have a higher priority and as long as these two parameters are in the input parameters, other input parameters will be ignored.
info
Unified account supports getting the past 730 days historical trades data
HTTP Request
GET /v5/execution/list

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse, spot, option
classic account: linear, inverse, spot
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
orderId	false	string	Order ID
orderLinkId	false	string	User customised order ID. Classic account does not support this param
baseCoin	false	string	Base coin, uppercase only
UTA1.0(cate  ry=inverse) and classic account are not supported
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 7 days by default;
Only startTime is passed, return range between startTime and startTime+7 days
Only endTime is passed, return range between endTime-7 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	integer	The end timestamp (ms)
execType	false	string	Execution type. Classic spot is not supported
limit	false	integer	Limit for data size per page. [1, 100]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> orderId	string	Order ID
> orderLinkId	string	User customized order ID. Classic spot is not supported
> side	string	Side. Buy,Sell
> orderPrice	string	Order price
> orderQty	string	Order qty
> leavesQty	string	The remaining qty not executed. Classic spot is not supported
> createType	string	Order create type
classic account & UTA1.0(cate  ry=inverse): always ""
Spot, Option do not have this key
> orderType	string	Order type. Market,Limit
> stopOrderType	string	Stop order type. If the order is not stop order, it either returns UNKNOWN or "". Classic spot is not supported
> execFee	string	Executed trading fee. You can get spot fee currency instruction here
> execFeeV2	string	Spot leg transaction fee, only works for execType=FutureSpread
> execId	string	Execution ID
> execPrice	string	Execution price
> execQty	string	Execution qty
> execType	string	Executed type. Classic spot is not supported
> execValue	string	Executed order value. Classic spot is not supported
> execTime	string	Executed timestamp (ms)
> feeCurrency	string	Spot trading fee currency Classic spot is not supported
> isMaker	boolean	Is maker order. true: maker, false: taker
> feeRate	string	Trading fee rate. Classic spot is not supported
> tradeIv	string	Implied volatility. Valid for option
> markIv	string	Implied volatility of mark price. Valid for option
> markPrice	string	The mark price of the symbol when executing. Classic spot is not supported
> indexPrice	string	The index price of the symbol when executing. Valid for option only
> underlyingPrice	string	The underlying price of the symbol when executing. Valid for option
> blockTradeId	string	Paradigm block trade ID
> closedSize	string	Closed position size
> seq	long	Cross sequence, used to associate each fill and each position update
The seq will be the same when conclude multiple transactions at the same time
Different symbols may have the same seq, please use seq + symbol to check unique
classic account Spot trade does not have this field
> extraFees	string	Trading fee rate information. Currently, this data is returned only for kyc=Indian user or spot orders placed on the Indonesian site or spot fiat currency orders placed on the EU site. In other cases, an empty string is returned. Enum: feeType, subFeeType
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
  
GET /v5/execution/list?cate  ry=linear&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672283754132
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "132766%3A2%2C132766%3A2",
        "cate  ry": "linear",
        "list": [
            {
                "symbol": "ETHPERP",
                "orderType": "Market",
                "underlyingPrice": "",
                "orderLinkId": "",
                "side": "Buy",
                "indexPrice": "",
                "orderId": "8c065341-7b52-4ca9-ac2c-37e31ac55c94",
                "stopOrderType": "UNKNOWN",
                "leavesQty": "0",
                "execTime": "1672282722429",
                "feeCurrency": "",
                "isMaker": false,
                "execFee": "0.071409",
                "feeRate": "0.0006",
                "execId": "e0cbe81d-0f18-5866-9415-cf319b5dab3b",
                "tradeIv": "",
                "blockTradeId": "",
                "markPrice": "1183.54",
                "execPrice": "1190.15",
                "markIv": "",
                "orderQty": "0.1",
                "orderPrice": "1236.9",
                "execValue": "119.015",
                "execType": "Trade",
                "execQty": "0.1",
                "closedSize": "",
                "extraFees": "",
                "seq": 4688002127
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672283754510
}


Batch Place Order
tip
This endpoint allows you to place more than one order in a single request.

Make sure you have sufficient funds in your account when placing an order. Once an order is placed, according to the funds required by the order, the funds in your account will be frozen by the corresponding amount during the life cycle of the order.
A maximum of 20 orders (option), 20 orders (inverse), 20 orders (linear), 10 orders (spot) can be placed per request. The returned data list is divided into two lists. The first list indicates whether or not the order creation was successful and the second list details the created order information. The structure of the two lists are completely consistent.
info
Option rate limt instruction: its rate limit is count based on the actual number of request sent, e.g., by default, option trading rate limit is 10 reqs per sec, so you can send up to 20 * 10 = 200 orders in one second.
Perpetual, Futures, Spot rate limit instruction, please check here
Risk control limit notice:
Bybit will monitor on your API requests. When the total number of orders of a single user (aggregated the number of orders across main account and subaccounts) within a day (UTC 0 - UTC 24) exceeds a certain upper limit, the platform will reserve the right to remind, warn, and impose necessary restrictions. Customers who use API default to acceptance of these terms and have the obligation to cooperate with adjustments.
HTTP Request
POST /v5/order/create-batch

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0: linear, option, spot, inverse
UTA1.0: linear, option, spot
request	true	array	Object
> symbol	true	string	Symbol name, like BTCUSDT, uppercase only
> isLeverage	false	integer	Whether to borrow. Valid for Unified spot only. 0(default): false then spot trading, 1: true then margin trading
> side	true	string	Buy, Sell
> orderType	true	string	Market, Limit
> qty	true	string	Order quantity
UTA account
Spot: set marketUnit for market order qty unit, quoteCoin for market buy by default, baseCoin for market sell by default
Perps, Futures & Option: always use base coin as unit
Perps & Futures: if you pass qty="0" and specify reduceOnly=true&closeOnTrigger=true, you can close the position up to maxMktOrderQty or maxOrderQty shown on Get Instruments Info of current symbol
> marketUnit	false	string	The unit for qty when create Spot market orders for UTA account, orderFilter=tpslOrder and StopOrder are supported as well.
baseCoin: for example, buy BTCUSDT, then "qty" unit is BTC
quoteCoin: for example, sell BTCUSDT, then "qty" unit is USDT
> price	false	string	Order price
Market order will ignore this field
Please check the min price and price precision from instrument info endpoint
If you have position, price needs to be better than liquidation price
> triggerDirection	false	integer	Conditional order param. Used to identify the expected direction of the conditional order.
1: triggered when market price rises to triggerPrice
2: triggered when market price falls to triggerPrice
Valid for linear
> orderFilter	false	string	If it is not passed, Order by default.
Order
tpslOrder: Spot TP/SL order, the assets are occupied even before the order is triggered
StopOrder: Spot conditional order, the assets will not be occupied until the price of the underlying asset reaches the trigger price, and the required assets will be occupied after the Conditional order is triggered
Valid for spot only
> triggerPrice	false	string	
For Perps & Futures, it is the conditional order trigger price. If you expect the price to rise to trigger your conditional order, make sure:
triggerPrice > market price
Else, triggerPrice < market price
For spot, it is the orderFilter=tpslOrder, or orderFilter=stopOrder trigger price
> triggerBy	false	string	Conditional order param (Perps & Futures). Trigger price type. LastPrice, IndexPrice, MarkPrice
> orderIv	false	string	Implied volatility. option only. Pass the real value, e.g for 10%, 0.1 should be passed. orderIv has a higher priority when price is passed as well
> timeInForce	false	string	Time in force
Market order will use IOC directly
If not passed, GTC is used by default
> positionIdx	false	integer	Used to identify positions in different position modes. Under hedge-mode, this param is required
0: one-way mode
1: hedge-mode Buy side
2: hedge-mode Sell side
> orderLinkId	false	string	User customised order ID. A max of 36 characters. Combinations of numbers, letters (upper and lower cases), dashes, and underscores are supported.
Futures, Perps & Spot: orderLinkId rules:
optional param
always unique
option orderLinkId rules:
required param
always unique
> takeProfit	false	string	Take profit price
> stopLoss	false	string	Stop loss price
> tpTriggerBy	false	string	The price type to trigger take profit. MarkPrice, IndexPrice, default: LastPrice.
Valid for linear, inverse
> slTriggerBy	false	string	The price type to trigger stop loss. MarkPrice, IndexPrice, default: LastPrice
Valid for linear, inverse
> reduceOnly	false	boolean	What is a reduce-only order? true means your position can only reduce in size if this order is triggered.
You must specify it as true when you are about to close/reduce the position
When reduceOnly is true, take profit/stop loss cannot be set
Valid for linear, inverse & option
> closeOnTrigger	false	boolean	What is a close on trigger order? For a closing order. It can only reduce your position, not increase it. If the account has insufficient available balance when the closing order is triggered, then other active orders of similar contracts will be cancelled or reduced. It can be used to ensure your stop loss reduces your position regardless of current available margin.
Valid for linear, inverse
> smpType	false	string	Smp execution type. What is SMP?
> mmp	false	boolean	Market maker protection. option only. true means set the order as a market maker protection order. What is mmp?
> tpslMode	false	string	TP/SL mode
Full: entire position for TP/SL. Then, tpOrderType or slOrderType must be Market
Partial: partial position tp/sl (as there is no size option, so it will create tp/sl orders with the qty you actually fill). Limit TP/SL order are supported. Note: When create limit tp/sl, tpslMode is required and it must be Partial
Valid for linear, inverse
> tpLimitPrice	false	string	The limit order price when take profit price is triggered
linear&inverse: only works when tpslMode=Partial and tpOrderType=Limit
Spot(UTA): it is required when the order has takeProfit and tpOrderType=Limit
> slLimitPrice	false	string	The limit order price when stop loss price is triggered
linear&inverse: only works when tpslMode=Partial and slOrderType=Limit
Spot(UTA): it is required when the order has stopLoss and slOrderType=Limit
> tpOrderType	false	string	The order type when take profit is triggered
linear&inverse: Market(default), Limit. For tpslMode=Full, it only supports tpOrderType=Market
Spot(UTA):
Market: when you set "takeProfit",
Limit: when you set "takeProfit" and "tpLimitPrice"
> slOrderType	false	string	The order type when stop loss is triggered
linear&inverse: Market(default), Limit. For tpslMode=Full, it only supports slOrderType=Market
Spot(UTA):
Market: when you set "stopLoss",
Limit: when you set "stopLoss" and "slLimitPrice"
Response Parameters
Parameter	Type	Comments
result	Object	
> list	array	Object
>> cate  ry	string	Product type
>> symbol	string	Symbol name
>> orderId	string	Order ID
>> orderLinkId	string	User customised order ID
>> createAt	string	Order created time (ms)
retExtInfo	Object	
> list	array	Object
>> code	number	Success/error code
>> msg	string	Success/error message
info
The acknowledgement of an place order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

RUN >>
Request Example
HTTP
 
  
  
.Net
  
POST /v5/order/create-batch HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672222064519
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "cate  ry": "spot",
    "request": [
        {
            "symbol": "BTCUSDT",
            "side": "Buy",
            "orderType": "Limit",
            "isLeverage": 0,
            "qty": "0.05",
            "price": "30000",
            "timeInForce": "GTC",
            "orderLinkId": "spot-btc-03"
        },
        {
            "symbol": "ATOMUSDT",
            "side": "Sell",
            "orderType": "Limit",
            "isLeverage": 0,
            "qty": "2",
            "price": "12",
            "timeInForce": "GTC",
            "orderLinkId": "spot-atom-03"
        }
    ]
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "cate  ry": "spot",
                "symbol": "BTCUSDT",
                "orderId": "1666800494330512128",
                "orderLinkId": "spot-btc-03",
                "createAt": "1713434102752"
            },
            {
                "cate  ry": "spot",
                "symbol": "ATOMUSDT",
                "orderId": "1666800494330512129",
                "orderLinkId": "spot-atom-03",
                "createAt": "1713434102752"
            }
        ]
    },
    "retExtInfo": {
        "list": [
            {
                "code": 0,
                "msg": "OK"
            },
            {
                "code": 0,
                "msg": "OK"
            }
        ]
    },
    "time": 1713434102753
}

Batch Amend Order
tip
This endpoint allows you to amend more than one open order in a single request.

You can modify unfilled or partially filled orders. Conditional orders are not supported.
A maximum of 20 orders (option), 20 orders (inverse), 20 orders (linear), 10 orders (spot) can be amended per request.
HTTP Request
POST /v5/order/amend-batch

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0: linear, option, spot, inverse
UTA1.0: linear, option, spot
request	true	array	Object
> symbol	true	string	Symbol name, like BTCUSDT, uppercase only
> orderId	false	string	Order ID. Either orderId or orderLinkId is required
> orderLinkId	false	string	User customised order ID. Either orderId or orderLinkId is required
> orderIv	false	string	Implied volatility. option only. Pass the real value, e.g for 10%, 0.1 should be passed
> triggerPrice	false	string	
For Perps & Futures, it is the conditional order trigger price. If you expect the price to rise to trigger your conditional order, make sure:
triggerPrice > market price
Else, triggerPrice < market price
For spot, it is for tpslOrder or stopOrder trigger price
> qty	false	string	Order quantity after modification. Do not pass it if not modify the qty
> price	false	string	Order price after modification. Do not pass it if not modify the price
> tpslMode	false	string	TP/SL mode
Full: entire position for TP/SL. Then, tpOrderType or slOrderType must be Market
Partial: partial position tp/sl. Limit TP/SL order are supported. Note: When create limit tp/sl, tpslMode is required and it must be Partial
> takeProfit	false	string	Take profit price after modification. If pass "0", it means cancel the existing take profit of the order. Do not pass it if you do not want to modify the take profit
> stopLoss	false	string	Stop loss price after modification. If pass "0", it means cancel the existing stop loss of the order. Do not pass it if you do not want to modify the stop loss
> tpTriggerBy	false	string	The price type to trigger take profit. When set a take profit, this param is required if no initial value for the order
> slTriggerBy	false	string	The price type to trigger stop loss. When set a take profit, this param is required if no initial value for the order
> triggerBy	false	string	Trigger price type
> tpLimitPrice	false	string	Limit order price when take profit is triggered. Only working when original order sets partial limit tp/sl
> slLimitPrice	false	string	Limit order price when stop loss is triggered. Only working when original order sets partial limit tp/sl
Response Parameters
Parameter	Type	Comments
result	Object	
> list	array	Object
>> cate  ry	string	Product type
>> symbol	string	Symbol name
>> orderId	string	Order ID
>> orderLinkId	string	User customised order ID
retExtInfo	Object	
> list	array	Object
>> code	number	Success/error code
>> msg	string	Success/error message
info
The acknowledgement of an amend order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

RUN >>
Request Example
HTTP
 
  
.Net
  
POST /v5/order/amend-batch HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672222935987
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "cate  ry": "option",
    "request": [
        {
            "symbol": "ETH-30DEC22-500-C",
            "qty": null,
            "price": null,
            "orderIv": "6.8",
            "orderId": "b551f227-7059-4fb5-a6a6-699c04dbd2f2"
        },
        {
            "symbol": "ETH-30DEC22-700-C",
            "qty": null,
            "price": "650",
            "orderIv": null,
            "orderId": "fa6a595f-1a57-483f-b9d3-30e9c8235a52"
        }
    ]
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "cate  ry": "option",
                "symbol": "ETH-30DEC22-500-C",
                "orderId": "b551f227-7059-4fb5-a6a6-699c04dbd2f2",
                "orderLinkId": ""
            },
            {
                "cate  ry": "option",
                "symbol": "ETH-30DEC22-700-C",
                "orderId": "fa6a595f-1a57-483f-b9d3-30e9c8235a52",
                "orderLinkId": ""
            }
        ]
    },
    "retExtInfo": {
        "list": [
            {
                "code": 0,
                "msg": "OK"
            },
            {
                "code": 0,
                "msg": "OK"
            }
        ]
    },
    "time": 1672222808060
}

Batch Cancel Order
This endpoint allows you to cancel more than one open order in a single request.

important
You must specify orderId or orderLinkId.
If orderId and orderLinkId is not matched, the system will process orderId first.
You can cancel unfilled or partially filled orders.
A maximum of 20 orders (option), 20 orders (inverse), 20 orders (linear), 10 orders (spot) can be cancelled per request.
HTTP Request
POST /v5/order/cancel-batch

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0: linear, option, spot, inverse
UTA1.0: linear, option, spot
request	true	array	Object
> symbol	true	string	Symbol name, like BTCUSDT, uppercase only
> orderId	false	string	Order ID. Either orderId or orderLinkId is required
> orderLinkId	false	string	User customised order ID. Either orderId or orderLinkId is required
Response Parameters
Parameter	Type	Comments
result	Object	
> list	array	Object
>> cate  ry	string	Product type
>> symbol	string	Symbol name
>> orderId	string	Order ID
>> orderLinkId	string	User customised order ID
retExtInfo	Object	
> list	array	Object
>> code	number	Success/error code
>> msg	string	Success/error message
info
The acknowledgement of an cancel order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

RUN >>
Request Example
HTTP
 
  
.Net
  
POST /v5/order/cancel-batch HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672223356634
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "cate  ry": "spot",
    "request": [
        {
            "symbol": "BTCUSDT",
            "orderId": "1666800494330512128"
        },
        {
            "symbol": "ATOMUSDT",
            "orderLinkId": "1666800494330512129"
        }
    ]
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "cate  ry": "spot",
                "symbol": "BTCUSDT",
                "orderId": "1666800494330512128",
                "orderLinkId": "spot-btc-03"
            },
            {
                "cate  ry": "spot",
                "symbol": "ATOMUSDT",
                "orderId": "",
                "orderLinkId": "1666800494330512129"
            }
        ]
    },
    "retExtInfo": {
        "list": [
            {
                "code": 0,
                "msg": "OK"
            },
            {
                "code": 170213,
                "msg": "Order does not exist."
            }
        ]
    },
    "time": 1713434299047
}

Get Borrow Quota (Spot)
Query the available balance for Spot trading and Margin trading

HTTP Request
GET /v5/order/spot-borrow-check

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: spot
symbol	true	string	Symbol name
side	true	string	Transaction side. Buy,Sell
Response Parameters
Parameter	Type	Comments
symbol	string	Symbol name, like BTCUSDT, uppercase only
side	string	Side
maxTradeQty	string	The maximum base coin qty can be traded
If spot margin trade on and symbol is margin trading pair, it returns available balance + max.borrowable quantity = min(The maximum quantity that a single user can borrow on the platform, The maximum quantity that can be borrowed calculated by IMR MMR of UTA account, The available quantity of the platform's capital pool)
Otherwise, it returns actual available balance
up to 4 decimals
maxTradeAmount	string	The maximum quote coin amount can be traded
If spot margin trade on and symbol is margin trading pair, it returns available balance + max.borrowable amount = min(The maximum amount that a single user can borrow on the platform, The maximum amount that can be borrowed calculated by IMR MMR of UTA account, The available amount of the platform's capital pool)
Otherwise, it returns actual available balance
up to 8 decimals
spotMaxTradeQty	string	No matter your Spot margin switch on or not, it always returns actual qty of base coin you can trade or you have (borrowable qty is not included), up to 4 decimals
spotMaxTradeAmount	string	No matter your Spot margin switch on or not, it always returns actual amount of quote coin you can trade or you have (borrowable amount is not included), up to 8 decimals
borrowCoin	string	Borrow coin
RUN >>
Request Example
HTTP
 
  
  
GET /v5/order/spot-borrow-check?cate  ry=spot&symbol=BTCUSDT&side=Buy HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672228522214
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "symbol": "BTCUSDT",
        "maxTradeQty": "6.6065",
        "side": "Buy",
        "spotMaxTradeAmount": "9004.75628594",
        "maxTradeAmount": "218014.01330797",
        "borrowCoin": "USDT",
        "spotMaxTradeQty": "0.2728"
    },
    "retExtInfo": {},
    "time": 1698895841534
}

Set Disconnect Cancel All
info
What is Disconnection Protect (DCP)?
Based on the websocket private connection and heartbeat mechanism, Bybit provides disconnection protection function. The timing starts from the first disconnection. If the Bybit server does not receive the reconnection from the client for more than 10 (default) seconds and resumes the heartbeat "ping", then the client is in the state of "disconnection protect", all active futures / spot / option orders of the client will be cancelled automatically. If within 10 seconds, the client reconnects and resumes the heartbeat "ping", the timing will be reset and restarted at the next disconnection.

How to enable DCP
If you need to turn it on/off, you can contact your client manager for consultation and application. The default time window is 10 seconds.

Applicable
Effective for Inverse Perp / Inverse Futures / USDT Perp / USDT Futures / USDC Perp / USDC Futures / Spot / options (UTA2.0)
Effective for USDT Perp / USDT Futures / USDC Perp / USDC Futures / Spot / options (UTA1.0)

tip
After the request is successfully sent, the system needs a certain time to take effect. It is recommended to query or set again after 10 seconds

You can use this endpoint to get your current DCP configuration.
Your private websocket connection must subscribe "dcp" topic in order to trigger DCP successfully
HTTP Request
POST /v5/order/disconnected-cancel-all

Request Parameters
Parameter	Required	Type	Comments
product	false	string	OPTIONS(default), DERIVATIVES, SPOT
timeWindow	true	integer	Disconnection timing window time. [3, 300], unit: second
Response Parameters
None

Request Example
HTTP
 
  
  
POST v5/order/disconnected-cancel-all HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675852742375
X-BAPI-RECV-WINDOW: 50000
Content-Type: application/json

{
  "timeWindow": 40
}

Response Example
{
    "retCode": 0,
    "retMsg": "success"
}

Pre Check Order
This endpoint is used to calculate the changes in IMR and MMR of UTA account before and after placing an order.

info
This endpoint supports orders with cate  ry = inverse,linear,option.
Only Cross Margin mode and Portfolio Margin mode are supported, isolated margin mode is not supported.
cate  ry = inverse is not supported in Cross Margin mode.
Conditional order is not supported.
HTTP Request
POST /v5/order/pre-check

Request Parameters
refer to create order request

Response Parameters
Parameter	Type	Comments
orderId	string	Order ID
orderLinkId	string	User customised order ID
preImrE4	int	Initial margin rate before checking, keep four decimal places. For examples, 30 means IMR = 30/1e4 = 0.30%
preMmrE4	int	Maintenance margin rate before checking, keep four decimal places. For examples, 30 means MMR = 30/1e4 = 0.30%
postImrE4	int	Initial margin rate calculated after checking, keep four decimal places. For examples, 30 means IMR = 30/1e4 = 0.30%
postMmrE4	int	Maintenance margin rate calculated after checking, keep four decimal places. For examples, 30 means MMR = 30/1e4 = 0.30%
Request Example
HTTP
 
  
POST /v5/order/pre-check HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672211928338
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

// Spot Limit order with market tp sl
{"cate  ry": "spot","symbol": "BTCUSDT","side": "Buy","orderType": "Limit","qty": "0.01","price": "28000","timeInForce": "PostOnly","takeProfit": "35000","stopLoss": "27000","tpOrderType": "Market","slOrderType": "Market"}

// Spot Limit order with limit tp sl
{"cate  ry": "spot","symbol": "BTCUSDT","side": "Buy","orderType": "Limit","qty": "0.01","price": "28000","timeInForce": "PostOnly","takeProfit": "35000","stopLoss": "27000","tpLimitPrice": "36000","slLimitPrice": "27500","tpOrderType": "Limit","slOrderType": "Limit"}

// Spot PostOnly normal order
{"cate  ry":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"0.1","price":"15600","timeInForce":"PostOnly","orderLinkId":"spot-test-01","isLeverage":0,"orderFilter":"Order"}

// Spot TP/SL order
{"cate  ry":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"0.1","price":"15600","triggerPrice": "15000", "timeInForce":"Limit","orderLinkId":"spot-test-02","isLeverage":0,"orderFilter":"tpslOrder"}

// Spot margin normal order (UTA)
{"cate  ry":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"0.1","price":"15600","timeInForce":"GTC","orderLinkId":"spot-test-limit","isLeverage":1,"orderFilter":"Order"}

// Spot Market Buy order, qty is quote currency
{"cate  ry":"spot","symbol":"BTCUSDT","side":"Buy","orderType":"Market","qty":"200","timeInForce":"IOC","orderLinkId":"spot-test-04","isLeverage":0,"orderFilter":"Order"}


// USDT Perp open long position (one-way mode)
{"cate  ry":"linear","symbol":"BTCUSDT","side":"Buy","orderType":"Limit","qty":"1","price":"25000","timeInForce":"GTC","positionIdx":0,"orderLinkId":"usdt-test-01","reduceOnly":false,"takeProfit":"28000","stopLoss":"20000","tpslMode":"Partial","tpOrderType":"Limit","slOrderType":"Limit","tpLimitPrice":"27500","slLimitPrice":"20500"}

// USDT Perp close long position (one-way mode)
{"cate  ry": "linear", "symbol": "BTCUSDT", "side": "Sell", "orderType": "Limit", "qty": "1", "price": "30000", "timeInForce": "GTC", "positionIdx": 0, "orderLinkId": "usdt-test-02", "reduceOnly": true}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderId": "24920bdb-4019-4e37-ad1c-876e3a855ac3",
        "orderLinkId": "test129",
        "preImrE4": 30,
        "preMmrE4": 21,
        "postImrE4": 357,
        "postMmrE4": 294
    },
    "retExtInfo": {},
    "time": 1749541599589
}

Get Position Info
Query real-time position data, such as position size, cumulative realized PNL, etc.

info
UTA2.0(inverse)

You can query all open positions with /v5/position/list?cate  ry=inverse;
Cannot query multiple symbols in one request
UTA1.0(inverse) & Classic (inverse)

You can query all open positions with /v5/position/list?cate  ry=inverse;
symbol parameter can pass up to 10 symbols, e.g., symbol=BTCUSD,ETHUSD
HTTP Request
GET /v5/position/list

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse, option
Classic account: linear, inverse
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
If symbol passed, it returns data regardless of having position or not.
If symbol=null and settleCoin specified, it returns position size greater than zero.
baseCoin	false	string	Base coin, uppercase only. option only. Return all option positions if not passed
settleCoin	false	string	Settle coin
linear: either symbol or settleCoin is required. symbol has a higher priority
limit	false	integer	Limit for data size per page. [1, 200]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
nextPageCursor	string	Refer to the cursor request parameter
list	array	Object
> positionIdx	integer	Position idx, used to identify positions in different position modes
0: One-Way Mode
1: Buy side of both side mode
2: Sell side of both side mode
> riskId	integer	Risk tier ID
for portfolio margin mode, this field returns 0, which means risk limit rules are invalid
> riskLimitValue	string	Risk limit value
for portfolio margin mode, this field returns 0, which means risk limit rules are invalid
> symbol	string	Symbol name
> side	string	Position side. Buy: long, Sell: short
one-way mode: classic & UTA1.0(inverse), an empty position returns None.
UTA2.0(linear, inverse) & UTA1.0(linear): either one-way or hedge mode returns an empty string "" for an empty position.
> size	string	Position size, always positive
> avgPrice	string	Average entry price
For USDC Perp & Futures, it indicates average entry price, and it will not be changed with 8-hour session settlement
> positionValue	string	Position value
> tradeMode	integer	Trade mode
Classic & UTA1.0(inverse): 0: cross-margin, 1: isolated margin
UTA2.0, UTA1.0(execpt inverse): deprecated, always 0, check Get Account Info to know the margin mode
> autoAddMargin	integer	Whether to add margin automatically when using isolated margin mode
0: false
1: true
> positionStatus	String	Position status. Normal, Liq, Adl
> leverage	string	Position leverage
for portfolio margin mode, this field returns "", which means leverage rules are invalid
> markPrice	string	Mark price
> liqPrice	string	Position liquidation price
UTA2.0(isolated margin), UTA1.0(isolated margin), UTA1.0(inverse), Classic account:
it is the real price for isolated and cross positions, and keeps "" when liqPrice <= minPrice or liqPrice >= maxPrice
UTA2.0(Cross margin), UTA1.0(Cross margin):
it is an estimated price for cross positions(because the unified mode controls the risk rate according to the account), and keeps "" when liqPrice <= minPrice or liqPrice >= maxPrice
this field is empty for Portfolio Margin Mode, and no liquidation price will be provided
> bustPrice	string	Bankruptcy price
> positionIM	string	Initial margin
Classic & UTA1.0(inverse): ignore this field
UTA portfolio margin mode, it returns ""
> positionIMByMp	string	Initial margin calculated by mark price
Classic & UTA1.0(inverse) : ignore this field
UTA portfolio margin mode, it returns ""
> positionMM	string	Maintenance margin
Classic & UTA1.0(inverse): ignore this field
UTA portfolio margin mode, it returns ""
> positionMMByMp	string	Maintenance margin calculated by mark price
Classic & UTA1.0(inverse) : ignore this field
UTA portfolio margin mode, it returns ""
> positionBalance	string	Position margin
Classic & UTA1.0(inverse) can refer to this field to get the position initial margin plus position closing fee
> takeProfit	string	Take profit price
> stopLoss	string	Stop loss price
> trailingStop	string	Trailing stop (The distance from market price)
> sessionAvgPrice	string	USDC contract session avg price, it is the same figure as avg entry price shown in the web UI
> delta	string	Delta
> gamma	string	Gamma
> vega	string	Vega
> theta	string	Theta
> unrealisedPnl	string	Unrealised PnL
> curRealisedPnl	string	The realised PnL for the current holding position
> cumRealisedPnl	string	Cumulative realised pnl
Futures & Perps: it is the all time cumulative realised P&L
Option: always "", meaningless
> adlRankIndicator	integer	Auto-deleverage rank indicator. What is Auto-Deleveraging?
> createdTime	string	Timestamp of the first time a position was created on this symbol (ms)
> updatedTime	string	Position updated timestamp (ms)
> seq	long	Cross sequence, used to associate each fill and each position update
Different symbols may have the same seq, please use seq + symbol to check unique
Returns "-1" if the symbol has never been traded
Returns the seq updated by the last transaction when there are settings like leverage, risk limit
> isReduceOnly	boolean	Useful when Bybit lower the risk limit
true: Only allowed to reduce the position. You can consider a series of measures, e.g., lower the risk limit, decrease leverage or reduce the position, add margin, or cancel orders, after these operations, you can call confirm new risk limit endpoint to check if your position can be removed the reduceOnly mark
false: There is no restriction, and it means your position is under the risk when the risk limit is systematically adjusted
Only meaningful for isolated margin & cross margin of USDT Perp, USDC Perp, USDC Futures, Inverse Perp and Inverse Futures, meaningless for others
> mmrSysUpdatedTime	string	Useful when Bybit lower the risk limit
When isReduceOnly=true: the timestamp (ms) when the MMR will be forcibly adjusted by the system
When isReduceOnly=false: the timestamp when the MMR had been adjusted by system
It returns the timestamp when the system operates, and if you manually operate, there is no timestamp
Keeps "" by default, if there was a lower risk limit system adjustment previously, it shows that system operation timestamp
Only meaningful for isolated margin & cross margin of USDT Perp, USDC Perp, USDC Futures, Inverse Perp and Inverse Futures, meaningless for others
> leverageSysUpdatedTime	string	Useful when Bybit lower the risk limit
When isReduceOnly=true: the timestamp (ms) when the leverage will be forcibly adjusted by the system
When isReduceOnly=false: the timestamp when the leverage had been adjusted by system
It returns the timestamp when the system operates, and if you manually operate, there is no timestamp
Keeps "" by default, if there was a lower risk limit system adjustment previously, it shows that system operation timestamp
Only meaningful for isolated margin & cross margin of USDT Perp, USDC Perp, USDC Futures, Inverse Perp and Inverse Futures, meaningless for others
> tpslMode	string	deprecated, always "Full"
RUN >>
Request Example
HTTP
 
  
  
GET /v5/position/list?cate  ry=inverse&symbol=BTCUSD HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672280218882
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "positionIdx": 0,
                "riskId": 1,
                "riskLimitValue": "150",
                "symbol": "BTCUSD",
                "side": "Sell",
                "size": "300",
                "avgPrice": "27464.50441675",
                "positionValue": "0.01092319",
                "tradeMode": 0,
                "positionStatus": "Normal",
                "autoAddMargin": 1,
                "adlRankIndicator": 2,
                "leverage": "10",
                "positionBalance": "0.00139186",
                "markPrice": "28224.50",
                "liqPrice": "",
                "bustPrice": "999999.00",
                "positionMM": "0.0000015",
                "positionMMByMp": "0.0000015",
                "positionIM": "0.00010923",
                "positionIMByMp": "0.00010923",
                "tpslMode": "Full",
                "takeProfit": "0.00",
                "stopLoss": "0.00",
                "trailingStop": "0.00",
                "unrealisedPnl": "-0.00029413",
                "curRealisedPnl": "0.00013123",
                "cumRealisedPnl": "-0.00096902",
                "seq": 5723621632,
                "isReduceOnly": false,
                "mmrSysUpdateTime": "",
                "leverageSysUpdatedTime": "",
                "sessionAvgPrice": "",
                "createdTime": "1676538056258",
                "updatedTime": "1697673600012"
            }
        ],
        "nextPageCursor": "",
        "cate  ry": "inverse"
    },
    "retExtInfo": {},
    "time": 1697684980172
}

Set Leverage
info
According to the risk limit, leverage affects the maximum position value that can be opened, that is, the greater the leverage, the smaller the maximum position value that can be opened, and vice versa. Learn more

HTTP Request
POST /v5/position/set-leverage

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse
Classic account: linear, inverse
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
buyLeverage	true	string	[1, max leverage]
one-way mode: buyLeverage must be the same as sellLeverage
Hedge mode:
Classic account & UTA (isolated margin): buyLeverage and sellLeverage can be different;
UTA (cross margin): buyLeverage must be the same as sellLeverage
sellLeverage	true	string	[1, max leverage]
RUN >>
Response Parameters
None

Request Example
HTTP
 
  
  
POST /v5/position/set-leverage HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672281605082
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "cate  ry": "linear",
    "symbol": "BTCUSDT",
    "buyLeverage": "6",
    "sellLeverage": "6"

}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtInfo": {},
    "time": 1672281607343
}

Switch Cross/Isolated Margin
Select cross margin mode or isolated margin mode per symbol level

HTTP Request
POST /v5/position/switch-isolated

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0: not supported
UTA1.0: inverse
Classic: linear(USDT Preps), inverse
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
tradeMode	true	integer	0: cross margin. 1: isolated margin
buyLeverage	true	string	The value must be equal to sellLeverage value
sellLeverage	true	string	The value must be equal to buyLeverage value
RUN >>
Response Parameters
None

Request Example
HTTP
 
  
  
POST /v5/position/switch-isolated HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN-TYPE: 2
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675248447965
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 121

{
    "cate  ry": "linear",
    "symbol": "ETHUSDT",
    "tradeMode": 1,
    "buyLeverage": "10",
    "sellLeverage": "10"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtInfo": {},
    "time": 1675248433635
}

Switch Position Mode
It supports to switch the position mode for USDT perpetual and Inverse futures. If you are in one-way Mode, you can only open one position on Buy or Sell side. If you are in hedge mode, you can open both Buy and Sell side positions simultaneously.

tip
Priority for configuration to take effect: symbol > coin > system default
System default: one-way mode
If the request is by coin (settleCoin), then all symbols based on this setteCoin that do not have position and open order will be batch switched, and new listed symbol based on this settleCoin will be the same mode you set.
Example
System default	coin	symbol
Initial setting	one-way	never configured	never configured
Result	All USDT perpetual trading pairs are one-way mode
Change 1	-	-	Set BTCUSDT to hedge-mode
Result	BTCUSDT becomes hedge-mode, and all other symbols keep one-way mode
list new symbol ETHUSDT	ETHUSDT is one-way mode (inherit default rules)
Change 2	-	Set USDT to hedge-mode	-
Result	All current trading pairs with no positions or orders are hedge-mode, and no adjustments will be made for trading pairs with positions and orders
list new symbol SOLUSDT	SOLUSDT is hedge-mode (Inherit coin rule)
Change 3	-	-	Set ASXUSDT to one-mode
Take effect result	AXSUSDT is one-way mode, other trading pairs have no change
list new symbol BITUSDT	BITUSDT is hedge-mode (Inherit coin rule)
The position-switch ability for each contract
Classic account	UTA1.0	UTA2.0
USDT perpetual	Support one-way & hedge-mode	Support one-way & hedge-mode	Support one-way & hedge-mode
USDT futures	N/A	Support one-way only	Support one-way only
USDC perpetual	N/A	Support one-way only	Support one-way only
USDC futures	N/A	Support one-way only	Support one-way only
Inverse perpetual	Support one-way only	Support one-way only	Support one-way only
Inverse futures	Support one-way & hedge-mode	Support one-way & hedge-mode	Support one-way only
HTTP Request
POST /v5/position/switch-mode

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0: linear, USDT Contract
UTA1.0: linear, USDT Contract; inverse, Inverse Futures
Classic: linear, USDT Perp; inverse, Inverse Futures
symbol	false	string	Symbol name, like BTCUSDT, uppercase only. Either symbol or coin is required. symbol has a higher priority
coin	false	string	Coin, uppercase only
mode	true	integer	Position mode. 0: Merged Single. 3: Both Sides
RUN >>
Response Parameters
None

Request Example
HTTP
 
  
  
POST /v5/position/switch-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675249072041
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 87

{
    "cate  ry":"inverse",
    "symbol":"BTCUSDH23",
    "coin": null,
    "mode": 0
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtInfo": {},
    "time": 1675249072814
}

Set Trading Stop
Set the take profit, stop loss or trailing stop for the position.

tip
Passing these parameters will create conditional orders by the system internally. The system will cancel these orders if the position is closed, and adjust the qty according to the size of the open position.

info
New version of TP/SL function supports both holding entire position TP/SL orders and holding partial position TP/SL orders.

Full position TP/SL orders: This API can be used to modify the parameters of existing TP/SL orders.
Partial position TP/SL orders: This API can only add partial position TP/SL orders.
note
Under the new version of TP/SL function, when calling this API to perform one-sided take profit or stop loss modification on existing TP/SL orders on the holding position, it will cause the paired tp/sl orders to lose binding relationship. This means that when calling the cancel API through the tp/sl order ID, it will only cancel the corresponding one-sided take profit or stop loss order ID.

HTTP Request
POST /v5/position/trading-stop

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse
Classic account: linear, inverse
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
tpslMode	true	string	TP/SL mode
Full: entire position TP/SL
Partial: partial position TP/SL
positionIdx	true	integer	Used to identify positions in different position modes.
0: one-way mode
1: hedge-mode Buy side
2: hedge-mode Sell side
takeProfit	false	string	Cannot be less than 0, 0 means cancel TP
stopLoss	false	string	Cannot be less than 0, 0 means cancel SL
trailingStop	false	string	Trailing stop by price distance. Cannot be less than 0, 0 means cancel TS
tpTriggerBy	false	string	Take profit trigger price type
slTriggerBy	false	string	Stop loss trigger price type
activePrice	false	string	Trailing stop trigger price. Trailing stop will be triggered when this price is reached only
tpSize	false	string	Take profit size
valid for TP/SL partial mode, note: the value of tpSize and slSize must equal
slSize	false	string	Stop loss size
valid for TP/SL partial mode, note: the value of tpSize and slSize must equal
tpLimitPrice	false	string	The limit order price when take profit price is triggered. Only works when tpslMode=Partial and tpOrderType=Limit
slLimitPrice	false	string	The limit order price when stop loss price is triggered. Only works when tpslMode=Partial and slOrderType=Limit
tpOrderType	false	string	The order type when take profit is triggered. Market(default), Limit
For tpslMode=Full, it only supports tpOrderType="Market"
slOrderType	false	string	The order type when stop loss is triggered. Market(default), Limit
For tpslMode=Full, it only supports slOrderType="Market"
Response Parameters
None

RUN >>
Request Example
HTTP
 
  
  
POST /v5/position/trading-stop HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672283124270
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "cate  ry":"linear",
    "symbol": "XRPUSDT",
    "takeProfit": "0.6",
    "stopLoss": "0.2",
    "tpTriggerBy": "MarkPrice",
    "slTriggerBy": "IndexPrice",
    "tpslMode": "Partial",
    "tpOrderType": "Limit",
    "slOrderType": "Limit",
    "tpSize": "50",
    "slSize": "50",
    "tpLimitPrice": "0.57",
    "slLimitPrice": "0.21",
    "positionIdx": 0
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtInfo": {},
    "time": 1672283125359
}

Set Auto Add Margin
Turn on/off auto-add-margin for isolated margin position

HTTP Request
POST /v5/position/set-auto-add-margin

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear (USDT Contract, USDC Contract)
Classic account: linear (USDT Perps)
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
autoAddMargin	true	integer	Turn on/off. 0: off. 1: on
positionIdx	false	integer	Used to identify positions in different position modes. For hedge mode position, this param is required
0: one-way mode
1: hedge-mode Buy side
2: hedge-mode Sell side
Response Parameters
None

RUN >>
Request Example
HTTP
 
  
  
POST /v5/position/set-auto-add-margin HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN-TYPE: 2
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675255134857
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "cate  ry": "linear",
    "symbol": "BTCUSDT",
    "autoAddmargin": 1,
    "positionIdx": null
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtInfo": {},
    "time": 1675255135069
}

Add Or Reduce Margin
Manually add or reduce margin for isolated margin position

HTTP Request
POST /v5/position/add-margin

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, inverse
Classic account: linear, inverse
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
margin	true	string	Add or reduce. To add, then 10; To reduce, then -10. Support up to 4 decimal
positionIdx	false	integer	Used to identify positions in different position modes. For hedge mode position, this param is required
0: one-way mode
1: hedge-mode Buy side
2: hedge-mode Sell side
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
symbol	string	Symbol name
positionIdx	integer	Position idx, used to identify positions in different position modes
0: One-Way Mode
1: Buy side of both side mode
2: Sell side of both side mode
riskId	integer	Risk limit ID
riskLimitValue	string	Risk limit value
size	string	Position size
avgPrice	string	Average entry price
liqPrice	string	Liquidation price
bustPrice	string	Bankruptcy price
markPrice	string	Last mark price
positionValue	string	Position value
leverage	string	Position leverage
autoAddMargin	integer	Whether to add margin automatically. 0: false, 1: true
positionStatus	String	Position status. Normal, Liq, Adl
positionIM	string	Initial margin
positionMM	string	Maintenance margin
takeProfit	string	Take profit price
stopLoss	string	Stop loss price
trailingStop	string	Trailing stop (The distance from market price)
unrealisedPnl	string	Unrealised PnL
cumRealisedPnl	string	Cumulative realised pnl
createdTime	string	Timestamp of the first time a position was created on this symbol (ms)
updatedTime	string	Position updated timestamp (ms)
RUN >>
Request Example
HTTP
 
  
  
POST /v5/position/add-margin HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1684234363665
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 97

{
    "cate  ry": "inverse",
    "symbol": "ETHUSD",
    "margin": "0.01",
    "positionIdx": 0
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "cate  ry": "inverse",
        "symbol": "ETHUSD",
        "positionIdx": 0,
        "riskId": 11,
        "riskLimitValue": "500",
        "size": "200",
        "positionValue": "0.11033265",
        "avgPrice": "1812.70004844",
        "liqPrice": "1550.80",
        "bustPrice": "1544.20",
        "markPrice": "1812.90",
        "leverage": "12",
        "autoAddMargin": 0,
        "positionStatus": "Normal",
        "positionIM": "0.01926611",
        "positionMM": "0",
        "unrealisedPnl": "0.00001217",
        "cumRealisedPnl": "-0.04618929",
        "stopLoss": "0.00",
        "takeProfit": "0.00",
        "trailingStop": "0.00",
        "createdTime": "1672737740039",
        "updatedTime": "1684234363788"
    },
    "retExtInfo": {},
    "time": 1684234363789
}

Get Closed PnL
Query user's closed profit and loss records

info
Classic account: the results are sorted by updatedTime in descending order.
UTA2.0, UTA1.0(except inverse): the results are sorted by createdTime in descending order, this will be constant with classic account afterwards
UTA2.0, UTA1.0(except inverse): support getting the past 730 days historical data
HTTP Request
GET /v5/position/closed-pnl

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear(USDT Contract, USDC Contract), inverse, option
Classic account: linear(USDT Perps), inverse
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 7 days by default
Only startTime is passed, return range between startTime and startTime+7 days
Only endTime is passed, return range between endTime-7 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 100]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> orderId	string	Order ID
> side	string	Buy, Sell
> qty	string	Order qty
> orderPrice	string	Order price
> orderType	string	Order type. Market,Limit
> execType	string	Exec type
Trade, BustTrade
SessionSettlePnL
Settle, MovePosition
> closedSize	string	Closed size
> cumEntryValue	string	Cumulated Position value
> avgEntryPrice	string	Average entry price
> cumExitValue	string	Cumulated exit position value
> avgExitPrice	string	Average exit price
> closedPnl	string	Closed PnL
> fillCount	string	The number of fills in a single order
> leverage	string	leverage
> openFee	string	Open position trading fee
> closeFee	string	Close position trading fee
> createdTime	string	The created time (ms)
> updatedTime	string	The updated time (ms)
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
  
GET /v5/position/closed-pnl?cate  ry=linear&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672284128523
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "5a373bfe-188d-4913-9c81-d57ab5be8068%3A1672214887231423699%2C5a373bfe-188d-4913-9c81-d57ab5be8068%3A1672214887231423699",
        "cate  ry": "linear",
        "list": [
            {
                "symbol": "ETHPERP",
                "orderType": "Market",
                "leverage": "3",
                "updatedTime": "1672214887236",
                "side": "Sell",
                "orderId": "5a373bfe-188d-4913-9c81-d57ab5be8068",
                "closedPnl": "-47.4065323",
                "avgEntryPrice": "1194.97516667",
                "qty": "3",
                "cumEntryValue": "3584.9255",
                "createdTime": "1672214887231",
                "orderPrice": "1122.95",
                "closedSize": "3",
                "avgExitPrice": "1180.59833333",
                "execType": "Trade",
                "fillCount": "4",
                "cumExitValue": "3541.795"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672284129153
}

Get Closed Options Positions
Query user's closed options positions, sorted by closeTime in descending order.

info
Only supports users to query closed options positions in the last 6 months.
Fee and price are displayed with trailing zeroes up to 8 decimal places.
HTTP Request
GET /v5/position/get-closed-positions

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	option
symbol	false	string	Symbol name
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 1 days by default
Only startTime is passed, return range between startTime and startTime+1 days
Only endTime is passed, return range between endTime-1 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 100]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
nextPageCursor	string	Refer to the cursor request parameter
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> side	string	Buy, Sell
> totalOpenFee	string	Total open fee
> deliveryFee	string	Delivery fee
> totalCloseFee	string	Total close fee
> qty	string	Order qty
> closeTime	integer	The closed time (ms)
> avgExitPrice	string	Average exit price
> deliveryPrice	string	Delivery price
> openTime	integer	The opened time (ms)
> avgEntryPrice	string	Average entry price
> totalPnl	string	Total PnL
Request Example
HTTP
GET /v5/position/get-closed-positions?cate  ry=option&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672284128523
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "1749726002161%3A0%2C1749715220240%3A1",
        "cate  ry": "option",
        "list": [
            {
                "symbol": "BTC-12JUN25-104019-C-USDT",
                "side": "Sell",
                "totalOpenFee": "0.94506647",
                "deliveryFee": "0.32184533",
                "totalCloseFee": "0.00000000",
                "qty": "0.02",
                "closeTime": 1749726002161,
                "avgExitPrice": "107281.77405000",
                "deliveryPrice": "107281.77405031",
                "openTime": 1749722990063,
                "avgEntryPrice": "3371.50000000",
                "totalPnl": "0.90760719"
            },
            {
                "symbol": "BTC-12JUN25-104000-C-USDT",
                "side": "Buy",
                "totalOpenFee": "0.86379999",
                "deliveryFee": "0.32287622",
                "totalCloseFee": "0.00000000",
                "qty": "0.02",
                "closeTime": 1749715220240,
                "avgExitPrice": "107625.40470150",
                "deliveryPrice": "107625.40470159",
                "openTime": 1749710568608,
                "avgEntryPrice": "3946.50000000",
                "totalPnl": "-7.60858218"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1749736532193
}

Move Position
You can move positions between sub-master, master-sub, or sub-sub UIDs when necessary

tip
UTA2.0 inverse contract move position is not supported for now

info
The endpoint can only be called by master UID api key
UIDs must be the same master-sub account relationship
The trades generated from move-position endpoint will not be displayed in the Recent Trade (Rest API & Websocket)
There is no trading fee
fromUid and toUid both should be Unified trading accounts, and they need to be one-way mode when moving the positions
Please note that once executed, you will get execType=MovePosition entry from Get Trade History, Get Closed Pnl, and stream from Execution.
HTTP Request
POST /v5/position/move-positions

Request Parameters
Parameter	Required	Type	Comments
fromUid	true	string	From UID
Must be UTA
Must be in one-way mode for Futures
toUid	true	string	To UID
Must be UTA
Must be in one-way mode for Futures
list	true	array	Object. Up to 25 legs per request
> cate  ry	true	string	Product type
UTA2.0, UTA1.0: linear, spot, option
> symbol	true	string	Symbol name, like BTCUSDT, uppercase only
> price	true	string	Trade price
linear: the price needs to be between [95% of mark price, 105% of mark price]
spot&option: the price needs to follow the price rule from Instruments Info
> side	true	string	Trading side of fromUid
For example, fromUid has a long position, when side=Sell, then once executed, the position of fromUid will be reduced or open a short position depending on qty input
> qty	true	string	Executed qty
The value must satisfy the qty rule from Instruments Info, in particular, cate  ry=linear is able to input maxOrderQty * 5
Response Parameters
Parameter	Type	Comments
retCode	integer	Result code. 0 means request is successfully accepted
retMsg	string	Result message
result	map	Object
> blockTradeId	string	Block trade ID
> status	string	Status. Processing, Rejected
> rejectParty	string	
"" means initial validation is passed, please check the order status via Get Move Position History
Taker, Maker when status=Rejected
bybit means error is occurred on the Bybit side
Request Example
HTTP
 
  
  
POST /v5/position/move-positions HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1697447928051
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "fromUid": "100307601",
    "toUid": "592324",
    "list": [
        {
            "cate  ry": "spot",
            "symbol": "BTCUSDT",
            "price": "100",
            "side": "Sell",
            "qty": "0.01"
        }
    ]
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "blockTradeId": "e9bb926c95f54cf1ba3e315a58b8597b",
        "status": "Processing",
        "rejectParty": ""
    }
}

Get Move Position History
You can query moved position data by master UID api key

info
UTA2.0 inverse contract move position is not supported for now

HTTP Request
GET /v5/position/move-history

Request Parameters
Parameter	Required	Type	Comments
cate  ry	false	string	Product type
UTA2.0, UTA1.0: linear, spot, option
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
startTime	false	number	The order creation start timestamp. The interval is 7 days
endTime	false	number	The order creation end timestamp. The interval is 7 days
status	false	string	Order status. Processing, Filled, Rejected
blockTradeId	false	string	Block trade ID
limit	false	string	Limit for data size per page. [1, 200]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> blockTradeId	string	Block trade ID
> cate  ry	string	Product type. linear, spot, option
> orderId	string	Bybit order ID
> userId	integer	User ID
> symbol	string	Symbol name
> side	string	Order side from taker's perspective. Buy, Sell
> price	string	Order price
> qty	string	Order quantity
> execFee	string	The fee for taker or maker in the base currency paid to the Exchange executing the block trade
> status	string	Block trade status. Processing, Filled, Rejected
> execId	string	The unique trade ID from the exchange
> resultCode	integer	The result code of the order. 0 means success
> resultMessage	string	The error message. "" when resultCode=0
> createdAt	number	The timestamp (ms) when the order is created
> updatedAt	number	The timestamp (ms) when the order is updated
> rejectParty	string	
"" means the status=Filled
Taker, Maker when status=Rejected
bybit means error is occurred on the Bybit side
nextPageCursor	string	Used to get the next page data
Request Example
HTTP
 
  
  
GET /v5/position/move-history?limit=1&status=Filled HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1697523024244
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "blockTradeId": "1a82e5801af74b67b7ad71ba00a7391a",
                "cate  ry": "option",
                "orderId": "8e09c5b8-f651-4cec-968d-52764cac11ec",
                "userId": 592324,
                "symbol": "BTC-14OCT23-27000-C",
                "side": "Buy",
                "price": "6",
                "qty": "0.99",
                "execFee": "0",
                "status": "Filled",
                "execId": "677ad344-6bb4-4ace-baca-128fcffcaca7",
                "resultCode": 0,
                "resultMessage": "",
                "createdAt": 1697186522865,
                "updatedAt": 1697186523289,
                "rejectParty": ""
            }
        ],
        "nextPageCursor": "page_token%3D1241742%26"
    },
    "retExtInfo": {},
    "time": 1697523024386
}

Confirm New Risk Limit
It is only applicable when the user is marked as only reducing positions (please see the isReduceOnly field in the Get Position Info interface). After the user actively adjusts the risk level, this interface is called to try to calculate the adjusted risk level, and if it passes (retCode=0), the system will remove the position reduceOnly mark. You are recommended to call Get Position Info to check isReduceOnly field.

HTTP Request
POST /v5/position/confirm-pending-mmr

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
Unified account: linear, inverse
Classic account: linear, inverse
symbol	true	string	Symbol name
Response Parameters
None

Request Example
HTTP
 
  
  
POST /v5/position/confirm-pending-mmr HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1698051123673
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 53

{
    "cate  ry": "linear",
    "symbol": "BTCUSDT"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtInfo": {},
    "time": 1698051124588
}

Set TP/SL Mode
tip
To some extent, this endpoint is deprecated because now tpsl is based on order level. This API was used for position level change before.

However, you still can use it to set an implicit tpsl mode for a certain symbol because when you don't pass "tpslMode" in the place order or trading stop request, system will get the tpslMode by the default setting.

Set TP/SL mode to Full or Partial

info
For partial TP/SL mode, you can set the TP/SL size smaller than position size.

HTTP Request
POST /v5/position/set-tpsl-mode

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
Unified account: linear, inverse
Classic account: linear, inverse. Please note that cate  ry is not involved with business logic
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
tpSlMode	true	string	TP/SL mode. Full,Partial
Response Parameters
Parameter	Type	Comments
tpSlMode	string	Full,Partial
RUN >>
Request Example
HTTP
 
  
  
POST /v5/position/set-tpsl-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672279325035
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "symbol": "XRPUSDT",
    "cate  ry": "linear",
    "tpSlMode": "Full"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "tpSlMode": "Full"
    },
    "retExtInfo": {},
    "time": 1672279322666
}

Set Risk Limit
Since bybit has launched auto risk limit on 12 March 2024, please click here to learn more, so it will not take effect even you set it successfully.

HTTP Request
POST /v5/position/set-risk-limit

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
Unified account: linear, inverse
Classic account: linear, inverse. Please note that cate  ry is not involved with business logic
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
riskId	true	integer	Risk limit ID
positionIdx	false	integer	Used to identify positions in different position modes. For hedge mode, it is required
0: one-way mode
1: hedge-mode Buy side
2: hedge-mode Sell side
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
riskId	integer	Risk limit ID
riskLimitValue	string	The position limit value corresponding to this risk ID
RUN >>
Request Example
HTTP
 
  
  
POST /v5/position/set-risk-limit HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672282269774
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "cate  ry": "linear",
    "symbol": "BTCUSDT",
    "riskId": 4,
    "positionIdx": null
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "riskId": 4,
        "riskLimitValue": "8000000",
        "cate  ry": "linear"
    },
    "retExtInfo": {},
    "time": 1672282270571
}

Get Pre-upgrade Order History
After the account is upgraded to a Unified account, you can get the orders which occurred before the upgrade.

UTA2.0:
By cate  ry=linear, you can query USDT Perps, USDC Perps data occurred during classic account
By cate  ry=spot, you can query Spot data occurred during classic account
By cate  ry=option, you can query Options data occurred during classic account
By cate  ry=inverse, you can query Inverse Contract data occurred during classic account or UTA1.0

UTA1.0:
By cate  ry=linear, you can query USDT Perps, USDC Perps data occurred during classic account
By cate  ry=spot, you can query Spot data occurred during classic account
By cate  ry=option, you can query Options data occurred during classic account

info
can get all status in 7 days
can only get filled orders beyond 7 days
USDC Perpeual & Option support the recent 6 months data. Please download older data via GUI
HTTP Request
GET /v5/pre-upgrade/order/history

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type linear, inverse, option, spot
symbol	false	string	Symbol name, like BTCUSDT, uppercase only.
If not passed, return settleCoin=USDT by default
To get USDC perp, please pass symbol
baseCoin	false	string	Base coin, uppercase only. Used for option query
orderId	false	string	Order ID
orderLinkId	false	string	User customised order ID
orderFilter	false	string	Order: active order, StopOrder: conditional order
orderStatus	false	string	Order status. Not supported for spot cate  ry
startTime	false	integer	The start timestamp (ms)
startTime and endTime must be passed together or both are not passed
endTime - startTime <= 7 days
If both are not passed, it returns recent 7 days by default
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> orderId	string	Order ID
> orderLinkId	string	User customised order ID
> blockTradeId	string	Block trade ID
> symbol	string	Symbol name
> price	string	Order price
> qty	string	Order qty
> side	string	Side. Buy,Sell
> isLeverage	string	Useless field for those orders before upgraded
> positionIdx	integer	Position index. Used to identify positions in different position modes
> orderStatus	string	Order status
> cancelType	string	Cancel type
> rejectReason	string	Reject reason
> avgPrice	string	Average filled price. If unfilled, it is "", and also for those orders have partilly filled but cancelled at the end
> leavesQty	string	The remaining qty not executed
> leavesValue	string	The estimated value not executed
> cumExecQty	string	Cumulative executed order qty
> cumExecValue	string	Cumulative executed order value
> cumExecFee	string	Cumulative executed trading fee
> timeInForce	string	Time in force
> orderType	string	Order type. Market,Limit
> stopOrderType	string	Stop order type
> orderIv	string	Implied volatility
> triggerPrice	string	Trigger price. If stopOrderType=TrailingStop, it is activate price. Otherwise, it is trigger price
> takeProfit	string	Take profit price
> stopLoss	string	Stop loss price
> tpTriggerBy	string	The price type to trigger take profit
> slTriggerBy	string	The price type to trigger stop loss
> triggerDirection	integer	Trigger direction. 1: rise, 2: fall
> triggerBy	string	The price type of trigger price
> lastPriceOnCreated	string	Last price when place the order
> reduceOnly	boolean	Reduce only. true means reduce position size
> closeOnTrigger	boolean	Close on trigger. What is a close on trigger order?
> placeType	string	Place type, option used. iv, price
> smpType	string	SMP execution type
> smpGroup	integer	Smp group ID. If the UID has no group, it is 0 by default
> smpOrderId	string	The counterparty's orderID which triggers this SMP execution
> createdTime	string	Order created timestamp (ms)
> updatedTime	string	Order updated timestamp (ms)
nextPageCursor	string	Refer to the cursor request parameter
Request Example
GET /v5/pre-upgrade/order/history?cate  ry=linear&limit=1&orderStatus=Filled HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1682576940304
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "orderId": "67836246-460e-4c52-a009-af0c3e1d12bc",
                "orderLinkId": "",
                "blockTradeId": "",
                "symbol": "BTCUSDT",
                "price": "27203.40",
                "qty": "0.200",
                "side": "Sell",
                "isLeverage": "",
                "positionIdx": 0,
                "orderStatus": "Filled",
                "cancelType": "UNKNOWN",
                "rejectReason": "EC_NoError",
                "avgPrice": "28632.126000",
                "leavesQty": "0.000",
                "leavesValue": "0",
                "cumExecQty": "0.200",
                "cumExecValue": "5726.4252",
                "cumExecFee": "3.43585512",
                "timeInForce": "IOC",
                "orderType": "Market",
                "stopOrderType": "UNKNOWN",
                "orderIv": "",
                "triggerPrice": "0.00",
                "takeProfit": "0.00",
                "stopLoss": "0.00",
                "tpTriggerBy": "UNKNOWN",
                "slTriggerBy": "UNKNOWN",
                "triggerDirection": 0,
                "triggerBy": "UNKNOWN",
                "lastPriceOnCreated": "0.00",
                "reduceOnly": true,
                "closeOnTrigger": true,
                "smpType": "None",
                "smpGroup": 0,
                "smpOrderId": "",
                "createdTime": "1682487465732",
                "updatedTime": "1682487465735",
                "placeType": ""
            }
        ],
        "nextPageCursor": "page_token%3D69406%26",
        "cate  ry": "linear"
    },
    "retExtInfo": {},
    "time": 1682576940540
}

Get Pre-upgrade Trade History
Get users' execution records which occurred before you upgraded the account to a Unified account, sorted by execTime in descending order

For now, it supports to query USDT perpetual, USDC perpetual, Inverse perpetual, Inverse futures, Spot and Option.

UTA2.0:
By cate  ry=linear, you can query USDT Perps, USDC Perps data occurred during classic account
By cate  ry=spot, you can query Spot data occurred during classic account
By cate  ry=option, you can query Options data occurred during classic account
By cate  ry=inverse, you can query Inverse Contract data occurred during classic account or UTA1.0

UTA1.0:
By cate  ry=linear, you can query USDT Perps, USDC Perps data occurred during classic account
By cate  ry=spot, you can query Spot data occurred during classic account
By cate  ry=option, you can query Options data occurred during classic account

info
USDC Perpeual & Option support the recent 6 months data. Please download older data via GUI

HTTP Request
GET /v5/pre-upgrade/execution/list

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type linear, inverse, option, spot
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
orderId	false	string	Order ID
orderLinkId	false	string	User customised order ID
baseCoin	false	string	Base coin, uppercase only. Used for option
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 7 days by default
Only startTime is passed, return range between startTime and startTime+7 days
Only endTime is passed, return range between endTime-7 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	integer	The end timestamp (ms)
execType	false	string	Execution type
limit	false	integer	Limit for data size per page. [1, 100]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> orderId	string	Order ID
> orderLinkId	string	User customized order ID
> side	string	Side. Buy,Sell
> orderPrice	string	Order price
> orderQty	string	Order qty
> leavesQty	string	The remaining qty not executed
> orderType	string	Order type. Market,Limit
> stopOrderType	string	Stop order type. If the order is not stop order, any type is not returned
> execFee	string	Executed trading fee
> execId	string	Execution ID
> execPrice	string	Execution price
> execQty	string	Execution qty
> execType	string	Executed type
> execValue	string	Executed order value
> execTime	string	Executed timestamp (ms)
> isMaker	boolean	Is maker order. true: maker, false: taker
> feeRate	string	Trading fee rate
> tradeIv	string	Implied volatility
> markIv	string	Implied volatility of mark price
> markPrice	string	The mark price of the symbol when executing
> indexPrice	string	The index price of the symbol when executing
> underlyingPrice	string	The underlying price of the symbol when executing
> blockTradeId	string	Paradigm block trade ID
> closedSize	string	Closed position size
nextPageCursor	string	Refer to the cursor request parameter
Request Example
GET /v5/pre-upgrade/execution/list?cate  ry=linear&limit=1&execType=Funding&symbol=BTCUSDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1682580752432
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "symbol": "BTCUSDT",
                "orderId": "1682553600-BTCUSDT-592334-Sell",
                "orderLinkId": "",
                "side": "Sell",
                "orderPrice": "0.00",
                "orderQty": "0.000",
                "leavesQty": "0.000",
                "orderType": "UNKNOWN",
                "stopOrderType": "UNKNOWN",
                "execFee": "0.6364003",
                "execId": "11f1c4ed-ff20-4d73-acb7-96e43a917f25",
                "execPrice": "28399.90",
                "execQty": "0.011",
                "execType": "Funding",
                "execValue": "312.3989",
                "execTime": "1682553600000",
                "isMaker": false,
                "feeRate": "0.00203714",
                "tradeIv": "",
                "markIv": "",
                "markPrice": "28399.90",
                "indexPrice": "",
                "underlyingPrice": "",
                "blockTradeId": "",
                "closedSize": "0.000"
            }
        ],
        "nextPageCursor": "page_token%3D96184191%26",
        "cate  ry": "linear"
    },
    "retExtInfo": {},
    "time": 1682580752717
}

Get Pre-upgrade Closed PnL
Query user's closed profit and loss records from before you upgraded the account to a Unified account. The results are sorted by updatedTime in descending order.

it only supports to query USDT perpetual, Inverse perpetual and Inverse Futures.

UTA2.0:
By cate  ry=linear, you can query USDT Perps data occurred during classic account
By cate  ry=inverse, you can query Inverse Contract data occurred during classic account or UTA1.0

UTA1.0:
By cate  ry=linear, you can query USDT Perps data occurred during classic account

HTTP Request
GET /v5/pre-upgrade/position/closed-pnl

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type linear, inverse
symbol	true	string	Symbol name, like BTCUSDT, uppercase only
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 7 days by default
Only startTime is passed, return range between startTime and startTime+7 days
Only endTime is passed, return range between endTime-7 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 100]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> orderId	string	Order ID
> side	string	Buy, Side
> qty	string	Order qty
> orderPrice	string	Order price
> orderType	string	Order type. Market,Limit
> execType	string	Exec type. Trade, BustTrade, SessionSettlePnL, Settle
> closedSize	string	Closed size
> cumEntryValue	string	Cumulated Position value
> avgEntryPrice	string	Average entry price
> cumExitValue	string	Cumulated exit position value
> avgExitPrice	string	Average exit price
> closedPnl	string	Closed PnL
> fillCount	string	The number of fills in a single order
> leverage	string	leverage
> createdTime	string	The created time (ms)
> updatedTime	string	The updated time (ms)
nextPageCursor	string	Refer to the cursor request parameter
Request Example
GET /v5/pre-upgrade/position/closed-pnl?cate  ry=linear&symbol=BTCUSDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1682580911998
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "symbol": "BTCUSDT",
                "orderId": "67836246-460e-4c52-a009-af0c3e1d12bc",
                "side": "Sell",
                "qty": "0.200",
                "orderPrice": "27203.40",
                "orderType": "Market",
                "execType": "Trade",
                "closedSize": "0.200",
                "cumEntryValue": "5588.88",
                "avgEntryPrice": "27944.40",
                "cumExitValue": "5726.4252",
                "avgExitPrice": "28632.13",
                "closedPnl": "204.25510011",
                "fillCount": "22",
                "leverage": "10",
                "createdTime": "1682487465732",
                "updatedTime": "1682487465732"
            }
        ],
        "cate  ry": "linear",
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1682580912259
}

Get Pre-upgrade Transaction Log
Query transaction logs which occurred in the USDC Derivatives wallet before the account was upgraded to a Unified account.

UTA2.0:
By cate  ry=linear, you can query USDC Perps transaction logs occurred during classic account By cate  ry=option, you can query Options transaction logs occurred during classic account

UTA1.0:
By cate  ry=linear, you can query USDC Perps transaction logs occurred during classic account By cate  ry=option, you can query Options transaction logs occurred during classic account

You can get USDC Perpetual, Option records.

info
USDC Perpeual & Option support the recent 6 months data. Please download older data via GUI

HTTP Request
GET /v5/pre-upgrade/account/transaction-log

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type linear, option
baseCoin	false	string	BaseCoin, uppercase only. e.g., BTC of BTCPERP
type	false	string	Types of transaction logs
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 7 days by default
Only startTime is passed, return range between startTime and startTime+7 days
Only endTime is passed, return range between endTime-7 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Used for pagination
Response Parameters
Parameter	Type	Comments
list	array	Object
> symbol	string	Symbol name
> cate  ry	string	Product type
> side	string	Side. Buy,Sell,None
> transactionTime	string	Transaction timestamp (ms)
> type	string	Type
> qty	string	Quantity
> size	string	Size
> currency	string	USDC、USDT、BTC、ETH
> tradePrice	string	Trade price
> funding	string	Funding fee
Positive value means receiving funding fee
Negative value means deducting funding fee
> fee	string	Trading fee
Positive fee value means expense
Negative fee value means rebates
> cashFlow	string	Cash flow
> change	string	Change
> cashBalance	string	Cash balance
> feeRate	string	
When type=TRADE, then it is trading fee rate
When type=SETTLEMENT, it means funding fee rate. For side=Buy, feeRate=market fee rate; For side=Sell, feeRate= - market fee rate
> bonusChange	string	The change of bonus
> tradeId	string	Trade ID
> orderId	string	Order ID
> orderLinkId	string	User customised order ID
nextPageCursor	string	Cursor. Used for pagination
Request Example
HTTP
 
GET /v5/pre-upgrade/account/transaction-log?cate  ry=option HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1686808288265
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "21%3A0%2C21%3A0",
        "list": [
            {
                "symbol": "ETH-14JUN23-1750-C",
                "side": "Buy",
                "funding": "",
                "orderLinkId": "",
                "orderId": "",
                "fee": "0",
                "change": "0",
                "cashFlow": "0",
                "transactionTime": "1686729604507",
                "type": "DELIVERY",
                "feeRate": "0",
                "bonusChange": "",
                "size": "0",
                "qty": "0.5",
                "cashBalance": "1001.1438885",
                "currency": "USDC",
                "cate  ry": "option",
                "tradePrice": "1740.25036667",
                "tradeId": ""
            }
        ]
    },
    "retExtInfo": {},
    "time": 1686809006792
}

Get Pre-upgrade Delivery Record
Query delivery records of Options before you upgraded the account to a Unified account, sorted by deliveryTime in descending order

UTA2.0:
By cate  ry=option, you can query Options delivery data occurred during classic account

UTA1.0:
By cate  ry=option, you can query Options delivery data occurred during classic account

info
Supports the recent 6 months Options delivery data. Please download older data via GUI

HTTP Request
GET /v5/pre-upgrade/asset/delivery-record

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type option
symbol	false	string	Symbol name, uppercase only
expDate	false	string	Expiry date. 25MAR22. Default: return all
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Used for pagination
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> deliveryTime	number	Delivery time (ms)
> symbol	string	Symbol name
> side	string	Buy,Sell
> position	string	Executed size
> deliveryPrice	string	Delivery price
> strike	string	Exercise price
> fee	string	Trading fee
> deliveryRpl	string	Realized PnL of the delivery
nextPageCursor	string	Cursor. Used for pagination
Request Example
HTTP
 
GET /v5/pre-upgrade/asset/delivery-record?cate  ry=option HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1686809005774
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "21%3A0%2C21%3A0",
        "cate  ry": "option",
        "list": [
            {
                "symbol": "ETH-14JUN23-1750-C",
                "side": "Buy",
                "deliveryTime": 1686729604507,
                "strike": "1750",
                "fee": "0",
                "position": "0.5",
                "deliveryPrice": "1740.25036667",
                "deliveryRpl": "0.175"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1686796328492
}

Get Pre-upgrade USDC Session Settlement
Query session settlement records of USDC perpetual before you upgrade the account to Unified account.

UTA2.0:
By cate  ry=option, you can query USDC Perps settlement data occurred during classic account

UTA1.0:
By cate  ry=option, you can query USDC Perps settlement data occurred during classic account

info
USDC Perpeual support the recent 6 months data. Please download older data via GUI

HTTP Request
GET /v5/pre-upgrade/asset/settlement-record

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type linear
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Used for pagination
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> side	string	Buy,Sell
> size	string	Position size
> sessionAvgPrice	string	Settlement price
> markPrice	string	Mark price
> realisedPnl	string	Realised PnL
> createdTime	string	Created time (ms)
nextPageCursor	string	Cursor. Used for pagination
Request Example
HTTP
 
GET /v5/pre-upgrade/asset/settlement-record?cate  ry=linear&symbol=ETHPERP&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1686809850982
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "25%3A0%2C25%3A0",
        "cate  ry": "linear",
        "list": [
            {
                "realisedPnl": "45.76",
                "symbol": "ETHPERP",
                "side": "Sell",
                "markPrice": "1668.44",
                "size": "-0.5",
                "createdTime": "1686787200000",
                "sessionAvgPrice": "1668.41"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1686809851749
}

Get Wallet Balance
Obtain wallet balance, query asset information of each currency. By default, currency information with assets or liabilities of 0 is not returned.

HTTP Request
GET /v5/account/wallet-balance

Request Parameters
Parameter	Required	Type	Comments
accountType	true	string	Account type
UTA2.0: UNIFIED
UTA1.0: UNIFIED, CONTRACT(inverse derivatives wallet)
Classic account: CONTRACT, SPOT
To get Funding wallet balance, please    to this endpoint
coin	false	string	Coin name, uppercase only
If not passed, it returns non-zero asset info
You can pass multiple coins to query, separated by comma. USDT,USDC
Response Parameters
Parameter	Type	Comments
list	array	Object
> accountType	string	Account type
> accountLTV	string	deprecated field
> accountIMRate	string	Account IM rate
You can refer to this Glossary to understand the below fields calculation and mearning
All account wide fields are not applicable to
UTA2.0(isolated margin),
UTA1.0(isolated margin), UTA1.0(CONTRACT),
classic account(SPOT, CONTRACT)
> accountIMRateByMp	string	Account IM rate calculated by mark price
> accountMMRate	string	Account MM rate
> accountMMRateByMp	string	Account MM rate calculated by mark price
> totalEquity	string	Account total equity (USD)
> totalWalletBalance	string	Account wallet balance (USD): ∑Asset Wallet Balance By USD value of each asset
> totalMarginBalance	string	Account margin balance (USD): totalWalletBalance + totalPerpUPL
> totalAvailableBalance	string	Account available balance (USD), Cross Margin: totalMarginBalance - totalInitialMargin
> totalPerpUPL	string	Account Perps and Futures unrealised p&l (USD): ∑Each Perp and USDC Futures upl by base coin
> totalInitialMargin	string	Account initial margin (USD): ∑Asset Total Initial Margin Base Coin
> totalInitialMarginByMp	string	Account initial margin (USD) calculated by mark price: ∑Asset Total Initial Margin Base Coin calculated by mark price
> totalMaintenanceMargin	string	Account maintenance margin (USD): ∑ Asset Total Maintenance Margin Base Coin
> totalMaintenanceMarginByMp	string	Account maintenance margin (USD) calculated by mark price: ∑ Asset Total Maintenance Margin Base Coin calculated by mark price
> coin	array	Object
>> coin	string	Coin name, such as BTC, ETH, USDT, USDC
>> equity	string	Equity of coin
>> usdValue	string	USD value of coin
>> walletBalance	string	Wallet balance of coin
>> free	string	Available balance for Spot wallet. This is a unique field for Classic SPOT
>> locked	string	Locked balance due to the Spot open order
>> spotHedgingQty	string	The spot asset qty that is used to hedge in the portfolio margin, truncate to 8 decimals and "0" by default
>> borrowAmount	string	Borrow amount of current coin
>> availableToWithdraw	string	Note: this field is deprecated for accountType=UNIFIED from 9 Jan, 2025
Transferable balance: you can use Get Transferable Amount (Unified) or Get All Coins Balance instead
Derivatives available balance:
isolated margin: walletBalance - totalPositionIM - totalOrderIM - locked - bonus
cross & portfolio margin: look at field totalAvailableBalance(USD), which needs to be converted into the available balance of accordingly coin through index price
Spot (margin) available balance: refer to Get Borrow Quota (Spot)
>> accruedInterest	string	Accrued interest
>> totalOrderIM	string	Pre-occupied margin for order. For portfolio margin mode, it returns ""
>> totalPositionIM	string	Sum of initial margin of all positions + Pre-occupied liquidation fee. For portfolio margin mode, it returns ""
>> totalPositionMM	string	Sum of maintenance margin for all positions. For portfolio margin mode, it returns ""
>> unrealisedPnl	string	Unrealised P&L
>> cumRealisedPnl	string	Cumulative Realised P&L
>> bonus	string	Bonus. This is a unique field for accounType=UNIFIED
>> marginCollateral	boolean	Whether it can be used as a margin collateral currency (platform), true: YES, false: NO
When marginCollateral=false, then collateralSwitch is meaningless
>> collateralSwitch	boolean	Whether the collateral is turned on by user (user), true: ON, false: OFF
When marginCollateral=true, then collateralSwitch is meaningful
>> availableToBorrow	string	deprecated field, always return "". Please refer to availableToBorrow in the Get Collateral Info
RUN >>
Request Example
HTTP
 
  
GET /v5/account/wallet-balance?accountType=UNIFIED&coin=BTC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672125440406
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "totalEquity": "3.31216591",
                "accountIMRate": "0",
                "accountIMRateByMp": "0",
                "totalMarginBalance": "3.00326056",
                "totalInitialMargin": "0",
                "totalInitialMarginByMp": "0",
                "accountType": "UNIFIED",
                "totalAvailableBalance": "3.00326056",
                "accountMMRate": "0",
                "accountMMRateByMp": "0",
                "totalPerpUPL": "0",
                "totalWalletBalance": "3.00326056",
                "accountLTV": "0",
                "totalMaintenanceMargin": "0",
                "totalMaintenanceMarginByMp": "0",
                "coin": [
                    {
                        "availableToBorrow": "3",
                        "bonus": "0",
                        "accruedInterest": "0",
                        "availableToWithdraw": "0",
                        "totalOrderIM": "0",
                        "equity": "0",
                        "totalPositionMM": "0",
                        "usdValue": "0",
                        "spotHedgingQty": "0.01592413",
                        "unrealisedPnl": "0",
                        "collateralSwitch": true,
                        "borrowAmount": "0.0",
                        "totalPositionIM": "0",
                        "walletBalance": "0",
                        "cumRealisedPnl": "0",
                        "locked": "0",
                        "marginCollateral": true,
                        "coin": "BTC"
                    }
                ]
            }
        ]
    },
    "retExtInfo": {},
    "time": 1690872862481
}

Get Transferable Amount (Unified)
Query the available amount to transfer of a specific coin in the Unified wallet.

HTTP Request
GET /v5/account/withdrawal

Request Parameters
Parameter	Required	Type	Comments
coinName	true	string	Coin name, uppercase only. Supports up to 20 coins per request, use comma to separate. BTC,USDC,USDT,SOL
Response Parameters
Parameter	Type	Comments
availableWithdrawal	string	Transferable amount for the 1st coin in the request
availableWithdrawalMap	Object	Transferable amount map for each requested coin. In the map, key is the requested coin, and value is the accordingly amount(string)
e.g., "availableWithdrawalMap":{"BTC":"4.54549050","SOL":"33.16713007","XRP":"10805.54548970","ETH":"17.76451865"}
Request Example
HTTP
 
  
GET /v5/account/withdrawal?coinName=BTC,SOL,ETH,XRP HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1739861239242
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "availableWithdrawal": "4.54549050",
        "availableWithdrawalMap": {
            "BTC": "4.54549050",
            "SOL": "33.16713007",
            "XRP": "10805.54548970",
            "ETH": "17.76451865"
        }
    },
    "retExtInfo": {},
    "time": 1739858984601
}

Upgrade to Unified Account
Upgrade Guidance
Check your current account status by calling this Get Account Info

if unifiedMarginStatus=1, then it is Classic account, you can call below upgrade endpoint to UTA2.0 Pro. Check Get Account Info after a while and if unifiedMarginStatus=6, then it is successfully upgraded to UTA2.0 Pro.

if unifiedMarginStatus=3, then it is UTA1.0, you have to head to website to click "upgrade" to UTA2.0 first.

if unifiedMarginStatus=4, then it is UTA1.0 Pro, you can call below upgrade endpoint to UTA2.0 Pro. Check Get Account Info after a while and if unifiedMarginStatus=6, then it is successfully upgraded to UTA2.0 Pro.

if unifiedMarginStatus=5, then it is UTA2.0, you can call below upgrade endpoint to UTA2.0 Pro. Check Get Account Info after a while and if unifiedMarginStatus=6, then it is successfully upgraded to UTA2.0 Pro.

important
Banned users cannot upgrade the account to Unified Account

info
You can upgrade the normal acct to unified acct without closing positions now, but please note belows:

Please avoid upgrading during these period:
every hour	50th minute to 5th minute of next hour
Please ensure: there is no open orders when upgrade from UTA2.0 to UTA2.0 Pro

Regaring the conditions that upgrade UTA1.0 Pro to UTA2.0 Pro, please ensure:
There is no open orders regardless of order types;
All inverse contract positions must keep consistent with the margin mode of Unified account. If it is Portfolio Margin mode, you either close inverse positions or switch unified account margin mode to cross or isolated margin mode.
Cannot have hedge mode inverse futures positions, which is not supported in UTA2.0
Cannot have TPSL order either
During the account upgrade process, the data of Rest API/Websocket stream may be inaccurate due to the fact that the account-related asset data is in the processing state. It is recommended to query and use it after the upgrade is completed.
HTTP Request
POST /v5/account/upgrade-to-uta

Request Parameters
None

Response Parameters
Parameter	Type	Comments
unifiedUpdateStatus	string	Upgrade status. FAIL,PROCESS,SUCCESS
unifiedUpdateMsg	Object	If PROCESS,SUCCESS, it returns null
> msg	array	Error message array. Only FAIL will have this field
RUN >>
Request Example
HTTP
 
  
  
.Net
  
POST /v5/account/upgrade-to-uta HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672125123533
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "unifiedUpdateStatus": "FAIL",
        "unifiedUpdateMsg": {
            "msg": [
                "Update account failed. You have outstanding liabilities in your Spot account.",
                "Update account failed. Please close the usdc perpetual positions in USDC Account.",
                "unable to upgrade, please cancel the usdt perpetual open orders in USDT account.",
                "unable to upgrade, please close the usdt perpetual positions in USDT account."
            ]
    }
},
    "retExtInfo": {},
    "time": 1672125124195
}

Get Borrow History
Get interest records, sorted in reverse order of creation time.

HTTP Request
GET /v5/account/borrow-history

Request Parameters
Parameter	Required	Type	Comments
currency	false	string	USDC,USDT,BTC,ETH etc, uppercase only
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 30 days by default
Only startTime is passed, return range between startTime and startTime + 30 days
Only endTime is passed, return range between endTime-30 days and endTime
If both are passed, the rule is endTime - startTime <= 30 days
endTime	false	integer	The end time. timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> currency	string	USDC,USDT,BTC,ETH
> createdTime	integer	Created timestamp (ms)
> borrowCost	string	Interest
> hourlyBorrowRate	string	Hourly Borrow Rate
> InterestBearingBorrowSize	string	Interest Bearing Borrow Size
> costExemption	string	Cost exemption
> borrowAmount	string	Total borrow amount
> unrealisedLoss	string	Unrealised loss
> freeBorrowedAmount	string	The borrowed amount for interest free
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
GET /v5/account/borrow-history?currency=BTC&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672277745427
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "2671153%3A1%2C2671153%3A1",
        "list": [
            {
                "borrowAmount": "1.06333265702840778",
                "costExemption": "0",
                "freeBorrowedAmount": "0",
                "createdTime": 1697439900204,
                "InterestBearingBorrowSize": "1.06333265702840778",
                "currency": "BTC",
                "unrealisedLoss": "0",
                "hourlyBorrowRate": "0.000001216904",
                "borrowCost": "0.00000129"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1697442206478
}

Repay Liability
You can manually repay the liabilities of Unified account

Permission: USDC Contracts

HTTP Request
POST /v5/account/quick-repayment

Request Parameters
Parameter	Required	Type	Comments
coin	false	string	The coin with liability, uppercase only
Input the specific coin: repay the liability of this coin in particular
No coin specified: repay the liability of all coins
Response Parameters
Parameter	Type	Comments
list	array	Object
> coin	string	Coin used for repayment
The order of currencies used to repay liability is based on liquidationOrder from this endpoint
> repaymentQty	string	Repayment qty
Request Example
HTTP
 
  
POST /v5/account/quick-repayment HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1701848610019
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 22

{
    "coin": "USDT"
}

Response Example
{
    "retCode": 0,
    "retMsg": "SUCCESS",
    "result": {
        "list": [
            {
                "coin": "BTC",
                "repaymentQty": "0.10549670"
            },
            {
                "coin": "ETH",
                "repaymentQty": "2.27768114"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1701848610941
}

Set Collateral Coin
You can decide whether the assets in the Unified account needs to be collateral coins.

HTTP Request
POST /v5/account/set-collateral-switch

Request Parameters
Parameter	Required	Type	Comments
coin	true	string	Coin name, uppercase only
You can get collateral coin from here
USDT, USDC cannot be set
collateralSwitch	true	string	ON: switch on collateral, OFF: switch off collateral
Response Parameters
None

RUN >>
Request Example
HTTP
 
  
POST /v5/account/set-collateral-switch HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1690513916181
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 55

{
    "coin": "BTC",
    "collateralSwitch": "ON"
}

Response Example
{
    "retCode": 0,
    "retMsg": "SUCCESS",
    "result": {},
    "retExtInfo": {},
    "time": 1690515818656
}

Batch Set Collateral Coin
HTTP Request
POST /v5/account/set-collateral-switch-batch

Request Parameters
Parameter	Required	Type	Comments
request	true	array	Object
> coin	true	string	Coin name, uppercase only
You can get collateral coin from here
USDT, USDC cannot be set
> collateralSwitch	true	string	ON: switch on collateral, OFF: switch off collateral
Response Parameters
Parameter	Type	Comments
result	Object	
> list	array	Object
>> coin	string	Coin name
>> collateralSwitch	string	ON: switch on collateral, OFF: switch off collateral
Request Example
HTTP
 
  
POST /v5/account/set-collateral-switch-batch HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1704782042755
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 371

{
    "request": [
        {
            "coin": "MATIC",
            "collateralSwitch": "OFF"
        },
        {
            "coin": "BTC",
            "collateralSwitch": "OFF"
        },
        {
            "coin": "ETH",
            "collateralSwitch": "OFF"
        },
        {
            "coin": "SOL",
            "collateralSwitch": "OFF"
        }
    ]
}

Response Example
{
    "retCode": 0,
    "retMsg": "SUCCESS",
    "result": {
        "list": [
            {
                "coin": "MATIC",
                "collateralSwitch": "OFF"
            },
            {
                "coin": "BTC",
                "collateralSwitch": "OFF"
            },
            {
                "coin": "ETH",
                "collateralSwitch": "OFF"
            },
            {
                "coin": "SOL",
                "collateralSwitch": "OFF"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1704782042913
}

Get Collateral Info
Get the collateral information of the current unified margin account, including loan interest rate, loanable amount, collateral conversion rate, whether it can be mortgaged as margin, etc.

HTTP Request
GET /v5/account/collateral-info

Request Parameters
Parameter	Required	Type	Comments
currency	false	string	Asset currency of all current collateral, uppercase only
Response Parameters
Parameter	Type	Comments
list	array	Object
> currency	string	Currency of all current collateral
> hourlyBorrowRate	string	Hourly borrow rate
> maxBorrowingAmount	string	Max borrow amount. This value is shared across main-sub UIDs
> freeBorrowingLimit	string	The maximum limit for interest-free borrowing
Only the borrowing caused by contracts unrealised loss has interest-free amount
Spot margin borrowing always has interest
> freeBorrowAmount	string	The amount of borrowing within your total borrowing amount that is exempt from interest charges
> borrowAmount	string	Borrow amount
> otherBorrowAmount	string	The sum of borrowing amount for other accounts under the same main account
> freeBorrowingAmount	string	deprecated field, always return "", please refer to freeBorrowingLimit
> availableToBorrow	string	Available amount to borrow. This value is shared across main-sub UIDs
> borrowable	boolean	Whether currency can be borrowed
> borrowUsageRate	string	Borrow usage rate: sum of main & sub accounts borrowAmount/maxBorrowingAmount, it is an actual value, 0.5 means 50%
> marginCollateral	boolean	Whether it can be used as a margin collateral currency (platform), true: YES, false: NO
When marginCollateral=false, then collateralSwitch is meaningless
> collateralSwitch	boolean	Whether the collateral is turned on by user (user), true: ON, false: OFF
When marginCollateral=true, then collateralSwitch is meaningful
> collateralRatio	string	Due to the new Tiered Collateral value logic, this field will no longer be accurate starting on February 19, 2025. Please refer to Get Tiered Collateral Ratio
RUN >>
Request Example
HTTP
 
  
GET /v5/account/collateral-info?currency=BTC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672127952719
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "availableToBorrow": "3",
                "freeBorrowingAmount": "",
                "freeBorrowAmount": "0",
                "maxBorrowingAmount": "3",
                "hourlyBorrowRate": "0.00000147",
                "borrowUsageRate": "0",
                "collateralSwitch": true,
                "borrowAmount": "0",
                "borrowable": true,
                "currency": "BTC",
                "otherBorrowAmount": "0",
                "marginCollateral": true,
                "freeBorrowingLimit": "0",
                "collateralRatio": "0.95"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1691565901952
}

Get Coin Greeks
Get current account Greeks information

HTTP Request
GET /v5/asset/coin-greeks

Request Parameters
Parameter	Required	Type	Comments
baseCoin	false	string	Base coin, uppercase only. If not passed, all supported base coin greeks will be returned by default
Response Parameters
Parameter	Type	Comments
list	array	Object
> baseCoin	string	Base coin. e.g.,BTC,ETH,SOL
> totalDelta	string	Delta value
> totalGamma	string	Gamma value
> totalVega	string	Vega value
> totalTheta	string	Theta value
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/coin-greeks?baseCoin=BTC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672287887610
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "baseCoin": "BTC",
                "totalDelta": "0.00004001",
                "totalGamma": "-0.00000009",
                "totalVega": "-0.00039689",
                "totalTheta": "0.01243824"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672287887942
}

Get Fee Rate
Get the trading fee rate.

HTTP Request
GET /v5/account/fee-rate

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type. spot, linear, inverse, option
symbol	false	string	Symbol name, like BTCUSDT, uppercase only. Valid for linear, inverse, spot
baseCoin	false	string	Base coin, uppercase only. SOL, BTC, ETH. Valid for option
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type. spot, option. Derivatives does not have this field
list	array	Object
> symbol	string	Symbol name. Keeps "" for Options
> baseCoin	string	Base coin. SOL, BTC, ETH
Derivatives does not have this field
Keeps "" for Spot
> takerFeeRate	string	Taker fee rate
> makerFeeRate	string	Maker fee rate
RUN >>
Request Example
HTTP
 
  
GET /v5/account/fee-rate?symbol=ETHUSDT HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676360412362
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "symbol": "ETHUSDT",
                "takerFeeRate": "0.0006",
                "makerFeeRate": "0.0001"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1676360412576
}

Get Account Info
Query the account information, like margin mode, account mode, etc.

HTTP Request
GET /v5/account/info

Request Parameters
None

Response Parameters
Parameter	Type	Comments
unifiedMarginStatus	integer	Account status
marginMode	string	ISOLATED_MARGIN, REGULAR_MARGIN, PORTFOLIO_MARGIN
isMasterTrader	boolean	Whether this account is a leader (copytrading). true, false
spotHedgingStatus	string	Whether the unified account enables Spot hedging. ON, OFF
updatedTime	string	Account data updated timestamp (ms)
dcpStatus	string	deprecated, always OFF. Please use Get DCP Info
timeWindow	integer	deprecated, always 0. Please use Get DCP Info
smpGroup	integer	deprecated, always 0. Please query Get SMP Group ID endpoint
RUN >>
Request Example
HTTP
 
  
GET /v5/account/info HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672129307221
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "marginMode": "REGULAR_MARGIN",
        "updatedTime": "1697078946000",
        "unifiedMarginStatus": 4,
        "dcpStatus": "OFF",
        "timeWindow": 10,
        "smpGroup": 0,
        "isMasterTrader": false,
        "spotHedgingStatus": "OFF"
    }
}

Get DCP Info
Query the DCP configuration of the account. Before calling the interface, please make sure you have applied for the UTA account DCP configuration with your account manager

Only the configured main / sub account can query information from this API. Calling this API by an account always returns empty.

If you only request to activate Spot trading for DCP, the contract and options data will not be returned.

info
Support UTA2.0:
USDT Perpetuals, USDT Futures, USDC Perpetuals, USDC Futures, Inverse Perpetuals, Inverse Futures [DERIVATIVES]
Spot [SPOT]
Options [OPTIONS]
Support UTA1.0:
USDT Perpetuals, USDT Futures, USDC Perpetuals, USDC Futures [DERIVATIVES]
Spot [SPOT]
Options [OPTIONS]
HTTP Request
GET /v5/account/query-dcp-info

Request Parameters
None

Response Parameters
Parameter	Type	Comments
dcpInfos	array<object>	DCP config for each product
> product	string	SPOT, DERIVATIVES, OPTIONS
> dcpStatus	string	Disconnected-CancelAll-Prevention status: ON
> timeWindow	string	DCP trigger time window which user pre-set. Between [3, 300] seconds, default: 10 sec
Request Example
HTTP
  
GET /v5/account/query-dcp-info HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1717065530867
X-BAPI-RECV-WINDOW: 5000

Response Example
// it means my account enables Spot and Deriviatvies on the backend
// Options is not enabled with DCP
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "dcpInfos": [
            {
                "product": "SPOT",
                "dcpStatus": "ON",
                "timeWindow": "10"
            },
            {
                "product": "DERIVATIVES",
                "dcpStatus": "ON",
                "timeWindow": "10"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1717065531697
}

Get Transaction Log
Query for transaction logs in your Unified account. It supports up to 2 years worth of data.

Apply to: UTA2.0, UTA1.0(except inverse)

HTTP Request
GET /v5/account/transaction-log

Request Parameters
Parameter	Required	Type	Comments
accountType	false	string	Account Type. UNIFIED
cate  ry	false	string	Product type
UTA2.0: spot,linear,option,inverse
UTA1.0: spot,linear,option
currency	false	string	Currency, uppercase only
baseCoin	false	string	BaseCoin, uppercase only. e.g., BTC of BTCPERP
type	false	string	Types of transaction logs
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 24 hours by default
Only startTime is passed, return range between startTime and startTime+24 hours
Only endTime is passed, return range between endTime-24 hours and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> id	string	Unique id
> symbol	string	Symbol name
> cate  ry	string	Product type
> side	string	Side. Buy,Sell,None
> transactionTime	string	Transaction timestamp (ms)
> type	string	Type
> transSubType	string	Transaction sub type, movePosition, used for the logs generated by move position. "" by default
> qty	string	Quantity
Spot: the negative means the qty of this currency is decreased, the positive means the qty of this currency is increased
Perps & Futures: it is the quantity for each trade entry and it does not have direction
> size	string	Size. The rest position size after the trade is executed, and it has direction, i.e., short with "-"
> currency	string	e.g., USDC, USDT, BTC, ETH
> tradePrice	string	Trade price
> funding	string	Funding fee
Positive fee value means an expense; negative fee value means a rebate. This is opposite to the execFee from Get Trade History.
For USDC Perp, as funding settlement and session settlement occur at the same time, they are represented in a single record at settlement. Please refer to funding to understand funding fee, and cashFlow to understand 8-hour P&L.
> fee	string	Trading fee
Positive fee value means expense
Negative fee value means rebates
> cashFlow	string	Cash flow, e.g., (1) close the position, and unRPL converts to RPL, (2) 8-hour session settlement for USDC Perp and Futures, (3) transfer in or transfer out. This does not include trading fee, funding fee
> change	string	Change = cashFlow + funding - fee
> cashBalance	string	Cash balance. This is the wallet balance after a cash change
> feeRate	string	
When type=TRADE, then it is trading fee rate
When type=SETTLEMENT, it means funding fee rate. For side=Buy, feeRate=market fee rate; For side=Sell, feeRate= - market fee rate
> bonusChange	string	The change of bonus
> tradeId	string	Trade ID
> orderId	string	Order ID
> orderLinkId	string	User customised order ID
> extraFees	string	Trading fee rate information. Currently, this data is returned only for spot orders placed on the Indonesian site or spot fiat currency orders placed on the EU site. In other cases, an empty string is returned. Enum: feeType, subFeeType
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
GET /v5/account/transaction-log?accountType=UNIFIED&cate  ry=linear&currency=USDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672132480085
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "21963%3A1%2C14954%3A1",
        "list": [
            {
                "transSubType": "",
                "id": "592324_XRPUSDT_161440249321",
                "symbol": "XRPUSDT",
                "side": "Buy",
                "funding": "-0.003676",
                "orderLinkId": "",
                "orderId": "1672128000-8-592324-1-2",
                "fee": "0.00000000",
                "change": "-0.003676",
                "cashFlow": "0",
                "transactionTime": "1672128000000",
                "type": "SETTLEMENT",
                "feeRate": "0.0001",
                "bonusChange": "",
                "size": "100",
                "qty": "100",
                "cashBalance": "5086.55825002",
                "currency": "USDT",
                "cate  ry": "linear",
                "tradePrice": "0.3676",
                "tradeId": "534c0003-4bf7-486f-aa02-78cee36825e4",
                "extraFees": ""
            },
            {
                "transSubType": "",
                "id": "592324_XRPUSDT_161440249321",
                "symbol": "XRPUSDT",
                "side": "Buy",
                "funding": "",
                "orderLinkId": "linear-order",
                "orderId": "592b7e41-78fd-42e2-9aa3-91e1835ef3e1",
                "fee": "0.01908720",
                "change": "-0.0190872",
                "cashFlow": "0",
                "transactionTime": "1672121182224",
                "type": "TRADE",
                "feeRate": "0.0006",
                "bonusChange": "-0.1430544",
                "size": "100",
                "qty": "88",
                "cashBalance": "5086.56192602",
                "currency": "USDT",
                "cate  ry": "linear",
                "tradePrice": "0.3615",
                "tradeId": "5184f079-88ec-54c7-8774-5173cafd2b4e",
                "extraFees": ""
            },
            {
                "transSubType": "",
                "id": "592324_XRPUSDT_161407743011",
                "symbol": "XRPUSDT",
                "side": "Buy",
                "funding": "",
                "orderLinkId": "linear-order",
                "orderId": "592b7e41-78fd-42e2-9aa3-91e1835ef3e1",
                "fee": "0.00260280",
                "change": "-0.0026028",
                "cashFlow": "0",
                "transactionTime": "1672121182224",
                "type": "TRADE",
                "feeRate": "0.0006",
                "bonusChange": "",
                "size": "12",
                "qty": "12",
                "cashBalance": "5086.58101322",
                "currency": "USDT",
                "cate  ry": "linear",
                "tradePrice": "0.3615",
                "tradeId": "8569c10f-5061-5891-81c4-a54929847eb3",
                "extraFees": ""
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672132481405
}

Get Transaction Log
Query transaction logs in the derivatives wallet (classic account), and inverse derivatives account (upgraded to UTA)

Permission: "Contract - Position"
Apply to: classic account, UTA1.0(inverse)

HTTP Request
GET /v5/account/contract-transaction-log

Request Parameters
Parameter	Required	Type	Comments
currency	false	string	Currency, uppercase only
baseCoin	false	string	BaseCoin, uppercase only. e.g., BTC of BTCPERP
type	false	string	Types of transaction logs
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 7 days by default
Only startTime is passed, return range between startTime and startTime+7 days
Only endTime is passed, return range between endTime-7 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> id	string	Unique id
> symbol	string	Symbol name
> cate  ry	string	Product type
> side	string	Side. Buy,Sell,None
> transactionTime	string	Transaction timestamp (ms)
> type	string	Type
> qty	string	Quantity
Perps & Futures: it is the quantity for each trade entry and it does not have direction
> size	string	Size. The rest position size after the trade is executed, and it has direction, i.e., short with "-"
> currency	string	currency
> tradePrice	string	Trade price
> funding	string	Funding fee
Positive value means deducting funding fee
Negative value means receiving funding fee
> fee	string	Trading fee
Positive fee value means expense
Negative fee value means rebates
> cashFlow	string	Cash flow, e.g., (1) close the position, and unRPL converts to RPL, (2) transfer in or transfer out. This does not include trading fee, funding fee
> change	string	Change = cashFlow - funding - fee
> cashBalance	string	Cash balance. This is the wallet balance after a cash change
> feeRate	string	
When type=TRADE, then it is trading fee rate
When type=SETTLEMENT, it means funding fee rate. For side=Buy, feeRate=market fee rate; For side=Sell, feeRate= - market fee rate
> bonusChange	string	The change of bonus
> tradeId	string	Trade ID
> orderId	string	Order ID
> orderLinkId	string	User customised order ID
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/account/contract-transaction-log?limit=1&symbol=BTCUSD HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1714035117255
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "id": "467153",
                "symbol": "BTCUSD",
                "cate  ry": "inverse",
                "side": "Sell",
                "transactionTime": "1714032000000",
                "type": "SETTLEMENT",
                "qty": "1000",
                "size": "-1000",
                "currency": "BTC",
                "tradePrice": "63974.88",
                "funding": "-0.00000156",
                "fee": "",
                "cashFlow": "0.00000000",
                "change": "0.00000156",
                "cashBalance": "1.1311",
                "feeRate": "-0.00010000",
                "bonusChange": "",
                "tradeId": "423a565c-f1b6-4c81-bc62-760cd7dd89e7",
                "orderId": "",
                "orderLinkId": ""
            }
        ],
        "nextPageCursor": "cursor_id%3D467153%26"
    },
    "retExtInfo": {},
    "time": 1714035117258
}

Get SMP Group ID
Query the SMP group ID of self match prevention

HTTP Request
GET /v5/account/smp-group

Request Parameters
None

Response Parameters
Parameter	Type	Comments
smpGroup	integer	Smp group ID. If the UID has no group, it is 0 by default
Request Example
HTTP
 
  
GET /v5/account/smp-group HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1702363848192
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "smpGroup": 0
    },
    "retExtInfo": {},
    "time": 1702363848539
}

Set Margin Mode
Default is regular margin mode

info
This switch does not work for the inverse trading in UTA1.0, which margin mode is set per symbol. Please use Switch Cross/Isolated Margin
HTTP Request
POST /v5/account/set-margin-mode

Request Parameters
Parameter	Required	Type	Comments
setMarginMode	true	string	ISOLATED_MARGIN, REGULAR_MARGIN(i.e. Cross margin), PORTFOLIO_MARGIN
Response Parameters
Parameter	Type	Comments
reasons	array	Object. If requested successfully, it is an empty array
> reasonCode	string	Fail reason code
> reasonMsg	string	Fail reason msg
RUN >>
Request Example
HTTP
 
  
POST /v5/account/set-margin-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672134396332
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "setMarginMode": "PORTFOLIO_MARGIN"
}

Response Example
{
    "retCode": 3400045,
    "retMsg": "Set margin mode failed",
    "result": {
        "reasons": [
            {
                "reasonCode": "3400000",
                "reasonMsg": "Equity needs to be equal to or greater than 1000 USDC"
            }
        ]
    }
}

Set Spot Hedging
You can turn on/off Spot hedging feature in Portfolio margin for Unified account

HTTP Request
POST /v5/account/set-hedging-mode

Request Parameters
Parameter	Required	Type	Comments
setHedgingMode	true	string	ON, OFF
Response Parameters
Parameter	Type	Comments
retCode	integer	Result code
retMsg	string	Result message
RUN >>
Request Example
HTTP
 
  
POST /v5/account/set-hedging-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1700117968580
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 31

{
    "setHedgingMode": "OFF"
}

Response Example
{
    "retCode": 0,
    "retMsg": "SUCCESS"
}

Set Limit Price Behaviour
You can configure how the system behaves when your limit order price exceeds the highest bid or lowest ask price.

Spot

Maximum Buy Price: Min[Max(Index, Index × (1 + y%) + 2-Minute Average Premium), Index × (1 + z%)]
Lowest price for Sell: Max[Min(Index, Index × (1 – y%) + 2-Minute Average Premium), Index × (1 – z%)]
Futures

Maximum Buy Price: min( max( index , markprice ( 1 + x% ）), markprice ( 1 + y%) )
Lowest price for Sell: max ( min( index , markprice ( 1 - x% )) , markprice ( 1 - y%) )
Default Setting
Spot: If the order price exceeds the boundary, the system rejects the request.

Futures: If the order price exceeds the boundary, the system will automatically adjust the price to the nearest allowed boundary (i.e., highest bid or lowest ask).

HTTP Request
POST /v5/account/set-limit-px-action

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	linear, inverse, spot
modifyEnable	true	boolean	true: allow the syetem to modify the order price
false: reject your order request
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/account/set-limit-px-action HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1753255927950
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 52

{
    "cate  ry": "spot",
    "modifyEnable": true
}

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {},
    "retExtInfo": {},
    "time": 1753255927952
}

Set MMP
info
What is MMP?
Market Maker Protection (MMP) is an automated mechanism designed to protect market makers (MM) against liquidity risks and over-exposure in the market. It prevents simultaneous trade executions on quotes provided by the MM within a short time span. The MM can automatically pull their quotes if the number of contracts traded for an underlying asset exceeds the configured threshold within a certain time frame. Once MMP is triggered, any pre-existing MMP orders will be automatically cancelled, and new orders tagged as MMP will be rejected for a specific duration — known as the frozen period — so that MM can reassess the market and modify the quotes.

How to enable MMP
Send an email to Bybit (financial.inst@bybit.com) or contact your business development (BD) manager to apply for MMP. After processed, the default settings are as below table:

Parameter	Type	Comments	Default value
baseCoin	string	Base coin	BTC
window	string	Time window (millisecond)	5000
frozenPeriod	string	Frozen period (millisecond)	100
qtyLimit	string	Quantity limit	100
deltaLimit	string	Delta limit	100
Applicable
Effective for options only. When you place an option order, set mmp=true, which means you mark this order as a mmp order.

Some points to note
Only maker order qty and delta will be counted into qtyLimit and deltaLimit.
qty_limit is the sum of absolute value of qty of each trade executions. delta_limit is the absolute value of the sum of qty*delta. If any of these reaches or exceeds the limit amount, the account's market maker protection will be triggered.
HTTP Request
POST /v5/account/mmp-modify

Request Parameters
Parameter	Required	Type	Comments
baseCoin	true	string	Base coin, uppercase only
window	true	string	Time window (ms)
frozenPeriod	true	string	Frozen period (ms). "0" means the trade will remain frozen until manually reset
qtyLimit	true	string	Trade qty limit (positive and up to 2 decimal places)
deltaLimit	true	string	Delta limit (positive and up to 2 decimal places)
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/account/mmp-modify HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675833524616
X-BAPI-RECV-WINDOW: 50000
Content-Type: application/json

{
    "baseCoin": "ETH",
    "window": "5000",
    "frozenPeriod": "100000",
    "qtyLimit": "50",
    "deltaLimit": "20"
}

Response Example
{
    "retCode": 0,
    "retMsg": "success"
}

Reset MMP
info
Once the mmp triggered, you can unfreeze the account by this endpoint, then qtyLimit and deltaLimit will be reset to 0.
If the account is not frozen, reset action can also remove previous accumulation, i.e., qtyLimit and deltaLimit will be reset to 0.
HTTP Request
POST /v5/account/mmp-reset

Request Parameters
Parameter	Required	Type	Comments
baseCoin	true	string	Base coin, uppercase only
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/account/mmp-reset HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675842997277
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "baseCoin": "ETH"
}

Response Example
{
    "retCode": 0,
    "retMsg": "success"
}

Get MMP State
HTTP Request
GET /v5/account/mmp-state

Request Parameters
Parameter	Required	Type	Comments
baseCoin	true	string	Base coin, uppercase only
Response Parameters
Parameter	Type	Comments
result	array	Object
> baseCoin	string	Base coin
> mmpEnabled	boolean	Whether the account is enabled mmp
> window	string	Time window (ms)
> frozenPeriod	string	Frozen period (ms)
> qtyLimit	string	Trade qty limit
> deltaLimit	string	Delta limit
> mmpFrozenUntil	string	Unfreeze timestamp (ms)
> mmpFrozen	boolean	Whether the mmp is triggered.
true: mmpFrozenUntil is meaningful
false: please ignore the value of mmpFrozenUntil
Request Example
HTTP
 
  
POST /v5/account/mmp-reset HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675842997277
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "baseCoin": "ETH"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "result": [
            {
                "baseCoin": "BTC",
                "mmpEnabled": true,
                "window": "5000",
                "frozenPeriod": "100000",
                "qtyLimit": "0.01",
                "deltaLimit": "0.01",
                "mmpFrozenUntil": "1675760625519",
                "mmpFrozen": false
            }
        ]
    },
    "retExtInfo": {},
    "time": 1675843188984
}

Get Delivery Record
Query delivery records of Invese Futures, USDC Futures, USDT Futures and Options, sorted by deliveryTime in descending order

HTTP Request
GET /v5/asset/delivery-record

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0: inverse(inverse futures), linear(USDT/USDC futures), option
UTA1.0: linear(USDT/USDC futures), option
symbol	false	string	Symbol name, like BTCUSDT, uppercase only
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 30 days by default
Only startTime is passed, return range between startTime and startTime + 30 days
Only endTime is passed, return range between endTime - 30 days and endTime
If both are passed, the rule is endTime - startTime <= 30 days
endTime	false	integer	The end timestamp (ms)
expDate	false	string	Expiry date. 25MAR22. Default: return all
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> deliveryTime	number	Delivery time (ms)
> symbol	string	Symbol name
> side	string	Buy,Sell
> position	string	Executed size
> entryPrice	string	Avg entry price
> deliveryPrice	string	Delivery price
> strike	string	Exercise price
> fee	string	Trading fee
> deliveryRpl	string	Realized PnL of the delivery
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/delivery-record?expDate=29DEC22&cate  ry=option HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672362112944
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "132791%3A0%2C132791%3A0",
        "cate  ry": "option",
        "list": [
            {
                "symbol": "BTC-29DEC22-16000-P",
                "side": "Buy",
                "deliveryTime": 1672300800860,
                "strike": "16000",
                "fee": "0.00000000",
                "position": "0.01",
                "deliveryPrice": "16541.86369547",
                "deliveryRpl": "3.5"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672362116184
}

Get USDC Session Settlement
Query session settlement records of USDC perpetual and futures

HTTP Request
GET /v5/asset/settlement-record

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	Product type
UTA2.0: linear(USDC contract)
UTA1.0: linear(USDC contract)
symbol	false	string	Symbol name, like BTCPERP, uppercase only
startTime	false	integer	The start timestamp (ms)
startTime and endTime are not passed, return 30 days by default
Only startTime is passed, return range between startTime and startTime + 30 days
Only endTime is passed, return range between endTime-30 days and endTime
If both are passed, the rule is endTime - startTime <= 30 days
endTime	false	integer	The end time. timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
cate  ry	string	Product type
list	array	Object
> symbol	string	Symbol name
> side	string	Buy,Sell
> size	string	Position size
> sessionAvgPrice	string	Settlement price
> markPrice	string	Mark price
> realisedPnl	string	Realised PnL
> createdTime	string	Created time (ms)
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/settlement-record?cate  ry=linear HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672284883483
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "116952%3A1%2C116952%3A1",
        "cate  ry": "linear",
        "list": [
            {
                "realisedPnl": "-71.28",
                "symbol": "BTCPERP",
                "side": "Buy",
                "markPrice": "16620",
                "size": "1.5",
                "createdTime": "1672214400000",
                "sessionAvgPrice": "16620"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1672284884285
}

Get Coin Exchange Records
Query the coin exchange records.

info
It sometimes has 5 secs delay

HTTP Request
GET /v5/asset/exchange/order-record

Request Parameters
Parameter	Required	Type	Comments
fromCoin	false	string	The currency to convert from, uppercase only. e.g,BTC
toCoin	false	string	The currency to convert to, uppercase only. e.g,USDT
limit	false	integer	Limit for data size per page. [1, 50]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
nextPageCursor	string	Refer to the cursor request parameter
orderBody	array	Object
> fromCoin	string	The currency to convert from
> fromAmount	string	The amount to convert from
> toCoin	string	The currency to convert to
> toAmount	string	The amount to convert to
> exchangeRate	string	Exchange rate
> createdTime	string	Exchange created timestamp (sec)
> exchangeTxId	string	Exchange transaction ID
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/exchange/order-record?limit=10 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672990462492
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderBody": [
            {
                "fromCoin": "BTC",
                "fromAmount": "0.100000000000000000",
                "toCoin": "ETH",
                "toAmount": "1.385866230000000000",
                "exchangeRate": "13.858662380000000000",
                "createdTime": "1672197760",
                "exchangeTxId": "145102533285208544812654440448"
            }
        ],
        "nextPageCursor": "173341:1672197760"
    },
    "retExtInfo": {},
    "time": 1672990464021
}

Get Coin Info
Query coin information, including chain information, withdraw and deposit status.

HTTP Request
GET /v5/asset/coin/query-info

Request Parameters
Parameter	Required	Type	Comments
coin	false	string	Coin, uppercase only
Response Parameters
Parameter	Type	Comments
rows	array	Object
> name	string	Coin name
> coin	string	Coin
> remainAmount	string	Maximum withdraw amount per transaction
> chains	array	Object
>> chain	string	Chain
>> chainType	string	Chain type
>> confirmation	string	Number of confirmations for deposit: Once this number is reached, your funds will be credited to your account and available for trading
>> withdrawFee	string	withdraw fee. If withdraw fee is empty, It means that this coin does not support withdrawal
>> depositMin	string	Min. deposit
>> withdrawMin	string	Min. withdraw
>> minAccuracy	string	The precision of withdraw or deposit
>> chainDeposit	string	The chain status of deposit. 0: suspend. 1: normal
>> chainWithdraw	string	The chain status of withdraw. 0: suspend. 1: normal
>> withdrawPercentageFee	string	The withdraw fee percentage. It is a real figure, e.g., 0.022 means 2.2%
>> contractAddress	string	Contract address. "" means no contract address
>> safeConfirmNumber	string	Number of security confirmations: Once this number is reached, your USD equivalent worth funds will be fully unlocked and available for withdrawal.
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/coin/query-info?coin=MNT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672194580887
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "rows": [
            {
                "name": "MNT",
                "coin": "MNT",
                "remainAmount": "10000000",
                "chains": [
                    {
                        "chainType": "Ethereum",
                        "confirmation": "6",
                        "withdrawFee": "3",
                        "depositMin": "0",
                        "withdrawMin": "3",
                        "chain": "ETH",
                        "chainDeposit": "1",
                        "chainWithdraw": "1",
                        "minAccuracy": "8",
                        "withdrawPercentageFee": "0",
                        "contractAddress": "0x3c3a81e81dc49a522a592e7622a7e711c06bf354",
                        "safeConfirmNumber": "65"
                    },
                    {
                        "chainType": "Mantle Network",
                        "confirmation": "100",
                        "withdrawFee": "0",
                        "depositMin": "0",
                        "withdrawMin": "10",
                        "chain": "MANTLE",
                        "chainDeposit": "1",
                        "chainWithdraw": "1",
                        "minAccuracy": "8",
                        "withdrawPercentageFee": "0",
                        "contractAddress": "",
                        "safeConfirmNumber": "100"
                    }
                ]
            }
        ]
    },
    "retExtInfo": {},
    "time": 1736395486989
}

Get Sub UID
Query the sub UIDs under a main UID. It returns up to 2000 sub accounts, if you need more, please call this endpoint.

info
Query by the master UID's api key only

HTTP Request
GET /v5/asset/transfer/query-sub-member-list

Request Parameters
None

Response Parameters
Parameter	Type	Comments
subMemberIds	array<string>	All sub UIDs under the main UID
transferableSubMemberIds	array<string>	All sub UIDs that have universal transfer enabled
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/transfer/query-sub-member-list HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672147239931
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "subMemberIds": [
            "554117",
            "592324",
            "592334",
            "1055262",
            "1072055",
            "1119352"
        ],
        "transferableSubMemberIds": [
            "554117",
            "592324"
        ]
    },
    "retExtInfo": {},
    "time": 1672147241320
}

Get Asset Info
Query Spot asset information

Apply to: classic account

HTTP Request
GET /v5/asset/transfer/query-asset-info

Request Parameters
Parameter	Required	Type	Comments
accountType	true	string	Account type. SPOT
coin	false	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
spot	Object	
> status	string	account status. ACCOUNT_STATUS_NORMAL: normal, ACCOUNT_STATUS_UNSPECIFIED: banned
> assets	array	Object
>> coin	string	Coin
>> frozen	string	Freeze amount
>> free	string	Free balance
>> withdraw	string	Amount in withdrawing
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/transfer/query-asset-info?accountType=SPOT&coin=ETH HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672136538042
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "spot": {
            "status": "ACCOUNT_STATUS_NORMAL",
            "assets": [
                {
                    "coin": "ETH",
                    "frozen": "0",
                    "free": "11.53485",
                    "withdraw": ""
                }
            ]
        }
    },
    "retExtInfo": {},
    "time": 1672136539127
}

Get All Coins Balance
You could get all coin balance of all account types under the master account, and sub account.

HTTP Request
GET /v5/asset/transfer/query-account-coins-balance

Request Parameters
Parameter	Required	Type	Comments
memberId	false	string	User Id. It is required when you use master api key to check sub account coin balance
accountType	true	string	Account type
coin	false	string	Coin name, uppercase only
Query all coins if not passed
Can query multiple coins, separated by comma. USDT,USDC,ETH
Note: this field is mandatory for accountType=UNIFIED, and supports up to 10 coins each request
withBonus	false	integer	0(default): not query bonus. 1: query bonus
Response Parameters
Parameter	Type	Comments
accountType	string	Account type
memberId	string	UserID
balance	array	Object
> coin	string	Currency
> walletBalance	string	Wallet balance
> transferBalance	string	Transferable balance
> bonus	string	Bonus
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/transfer/query-account-coins-balance?accountType=FUND&coin=USDC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675866354698
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "memberId": "XXXX",
        "accountType": "FUND",
        "balance": [
            {
                "coin": "USDC",
                "transferBalance": "0",
                "walletBalance": "0",
                "bonus": ""
            }
        ]
    },
    "retExtInfo": {},
    "time": 1675866354913
}

Get Single Coin Balance
Query the balance of a specific coin in a specific account type. Supports querying sub UID's balance. Also, you can check the transferable amount from master to sub account, sub to master account or sub to sub account, especially for user who has an institutional loan.

HTTP Request
GET /v5/asset/transfer/query-account-coin-balance

Request Parameters
Parameter	Required	Type	Comments
memberId	false	string	UID. Required when querying sub UID balance with master api key
toMemberId	false	string	UID. Required when querying the transferable balance between different UIDs
accountType	true	string	Account type
toAccountType	false	string	To account type. Required when querying the transferable balance between different account types
coin	true	string	Coin, uppercase only
withBonus	false	integer	0(default): not query bonus. 1: query bonus
withTransferSafeAmount	false	integer	Whether query delay withdraw/transfer safe amount
0(default): false, 1: true
What is delay withdraw amount?
withLtvTransferSafeAmount	false	integer	For OTC loan users in particular, you can check the transferable amount under risk level
0(default): false, 1: true
toAccountType is mandatory
Response Parameters
Parameter	Type	Comments
accountType	string	Account type
bizType	integer	Biz type
accountId	string	Account ID
memberId	string	Uid
balance	Object	
> coin	string	Coin
> walletBalance	string	Wallet balance
> transferBalance	string	Transferable balance
> bonus	string	bonus
> transferSafeAmount	string	Safe amount to transfer. Keep "" if not query
> ltvTransferSafeAmount	string	Transferable amount for ins loan account. Keep "" if not query
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/transfer/query-account-coin-balance?accountType=UNIFIED&coin=USDT&toAccountType=FUND&withLtvTransferSafeAmount=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: xxxxx
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1690254520644
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "accountType": "UNIFIED",
        "bizType": 1,
        "accountId": "1631385",
        "memberId": "1631373",
        "balance": {
            "coin": "USDT",
            "walletBalance": "11999",
            "transferBalance": "11999",
            "bonus": "0",
            "transferSafeAmount": "",
            "ltvTransferSafeAmount": "7602.4861"
        }
    },
    "retExtInfo": {},
    "time": 1690254521256
}

Get Withdrawable Amount
info
How can partial funds be subject to delayed withdrawal requests?

On-chain deposit: If the number of on-chain confirmations has not reached a risk-controlled level, a portion of the funds will be frozen for a period of time until they are unfrozen.
Buying crypto: If there is a risk, the funds will be frozen for a certain period of time and cannot be withdrawn.
HTTP Request
GET /v5/asset/withdraw/withdrawable-amount

Request Parameters
Parameter	Required	Type	Comments
coin	true	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
limitAmountUsd	string	The frozen amount due to risk, in USD
withdrawableAmount	Object	
> SPOT	Object	Spot wallet, it is not returned if spot wallet is removed
>> coin	string	Coin name
>> withdrawableAmount	string	Amount that can be withdrawn
>> availableBalance	string	Available balance
> FUND	Object	Funding wallet
>> coin	string	Coin name
>> withdrawableAmount	string	Amount that can be withdrawn
>> availableBalance	string	Available balance
> UTA	Object	Unified wallet
>> coin	string	Coin name
>> withdrawableAmount	string	Amount that can be withdrawn
>> availableBalance	string	Available balance
Request Example
HTTP
 
  
GET /v5/asset/withdraw/withdrawable-amount?coin=USDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1677565621998
X-BAPI-RECV-WINDOW: 50000
X-BAPI-SIGN: XXXXXX

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "limitAmountUsd": "595051.7",
        "withdrawableAmount": {
            "FUND": {
                "coin": "USDT",
                "withdrawableAmount": "155805.847",
                "availableBalance": "155805.847"
            },
            "UTA": {
                "coin": "USDT",
                "withdrawableAmount": "498751.0882",
                "availableBalance": "498751.0882"
            }
        }
    },
    "retExtInfo": {},
    "time": 1754009688289
}

Get Transferable Coin
Query the transferable coin list between each account type

HTTP Request
GET /v5/asset/transfer/query-transfer-coin-list

Request Parameters
Parameter	Required	Type	Comments
fromAccountType	true	string	From account type
toAccountType	true	string	To account type
Response Parameters
Parameter	Type	Comments
list	array	A list of coins (as strings)
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/transfer/query-transfer-coin-list?fromAccountType=UNIFIED&toAccountType=CONTRACT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672144322595
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "list": [
            "BTC",
            "ETH"
        ]
    },
    "retExtInfo": {},
    "time": 1672144322954
}

Create Internal Transfer
Create the internal transfer between different account types under the same UID.

tip
Please refer to transferable coin list API to find out more.
HTTP Request
POST /v5/asset/transfer/inter-transfer

Request Parameters
Parameter	Required	Type	Comments
transferId	true	string	UUID. Please manually generate a UUID
coin	true	string	Coin, uppercase only
amount	true	string	Amount
fromAccountType	true	string	From account type
toAccountType	true	string	To account type
Response Parameters
Parameter	Type	Comments
transferId	string	UUID
status	string	Transfer status
STATUS_UNKNOWN
SUCCESS
PENDING
FAILED
RUN >>
Request Example
HTTP
 
  
POST v5/asset/transfer/inter-transfer HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1670986690556
X-BAPI-RECV-WINDOW: 50000
X-BAPI-SIGN: XXXXX
Content-Type: application/json
{
    "transferId": "42c0cfb0-6bca-c242-bc76-4e6df6cbcb16",
    "coin": "BTC",
    "amount": "0.05",
    "fromAccountType": "UNIFIED",
    "toAccountType": "CONTRACT"
}

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "transferId": "42c0cfb0-6bca-c242-bc76-4e6df6cbab16",
        "status": "SUCCESS"
    },
    "retExtInfo": {},
    "time": 1670986962783
}

Get Internal Transfer Records
Query the internal transfer records between different account types under the same UID.

info
If startTime and endTime are not provided, the API returns data from the past 7 days by default.
If only startTime is provided, the API returns records from startTime to startTime + 7 days.
If only endTime is provided, the API returns records from endTime - 7 days to endTime.
If both are provided, the maximum allowed range is 7 days (endTime - startTime ≤ 7 days).
HTTP Request
GET /v5/asset/transfer/query-inter-transfer-list

Request Parameters
Parameter	Required	Type	Comments
transferId	false	string	UUID. Use the one you generated in createTransfer
coin	false	string	Coin, uppercase only
status	false	string	Transfer status
startTime	false	integer	The start timestamp (ms) Note: the query logic is actually effective based on second level
endTime	false	integer	The end timestamp (ms) Note: the query logic is actually effective based on second level
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> transferId	string	Transfer ID
> coin	string	Transferred coin
> amount	string	Transferred amount
> fromAccountType	string	From account type
> toAccountType	string	To account type
> timestamp	string	Transfer created timestamp (ms)
> status	string	Transfer status
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/transfer/inter-transfer-list-query?coin=USDT&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1670988271299
X-BAPI-RECV-WINDOW: 50000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
    "list": [
        {
            "transferId": "selfTransfer_a1091cc7-9364-4b74-8de1-18f02c6f2d5c",
            "coin": "USDT",
            "amount": "5000",
            "fromAccountType": "SPOT",
            "toAccountType": "UNIFIED",
            "timestamp": "1667283263000",
            "status": "SUCCESS"
        }
    ],
    "nextPageCursor": "eyJtaW5JRCI6MTM1ODQ2OCwibWF4SUQiOjEzNTg0Njh9"
},
    "retExtInfo": {},
    "time": 1670988271677
}

Create Universal Transfer
Transfer between sub-sub or main-sub.

tip
Use master or sub acct api key to request
To use sub acct api key, it must have "SubMemberTransferList" permission
When use sub acct api key, it can only transfer to main account
If you encounter errorCode: 131228 and msg: your balance is not enough, please    to Get Single Coin Balance to check transfer safe amount.
You can not transfer between the same UID.
Currently, the funding wallet only supports out  ing transfers in cryptocurrency, not in fiat currency.
HTTP Request
POST /v5/asset/transfer/universal-transfer

Request Parameters
Parameter	Required	Type	Comments
transferId	true	string	UUID. Please manually generate a UUID
coin	true	string	Coin, uppercase only
amount	true	string	Amount
fromMemberId	true	integer	From UID
toMemberId	true	integer	To UID
fromAccountType	true	string	From account type
toAccountType	true	string	To account type
Response Parameters
Parameter	Type	Comments
transferId	string	UUID
status	string	Transfer status
STATUS_UNKNOWN
SUCCESS
PENDING
FAILED
RUN >>
Request Example
HTTP
 
  
POST /v5/asset/transfer/universal-transfer HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672189449697
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
Content-Type: application/json

{
    "transferId": "be7a2462-1138-4e27-80b1-62653f24925e",
    "coin": "ETH",
    "amount": "0.5",
    "fromMemberId": 592334,
    "toMemberId": 691355,
    "fromAccountType": "CONTRACT",
    "toAccountType": "UNIFIED"

}

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "transferId": "be7a2462-1138-4e27-80b1-62653f24925e",
        "status": "SUCCESS"
    },
    "retExtInfo": {},
    "time": 1672189450195
}

Get Universal Transfer Records
Query universal transfer records

tip
Main acct api key or Sub acct api key are both supported
Main acct api key needs "SubMemberTransfer" permission
Sub acct api key needs "SubMemberTransferList" permission
info
If startTime and endTime are not provided, the API returns data from the past 7 days by default.
If only startTime is provided, the API returns records from startTime to startTime + 7 days.
If only endTime is provided, the API returns records from endTime - 7 days to endTime.
If both are provided, the maximum allowed range is 7 days (endTime - startTime ≤ 7 days).
HTTP Request
GET /v5/asset/transfer/query-universal-transfer-list

Request Parameters
Parameter	Required	Type	Comments
transferId	false	string	UUID. Use the one you generated in createTransfer
coin	false	string	Coin, uppercase only
status	false	string	Transfer status. SUCCESS,FAILED,PENDING
startTime	false	integer	The start timestamp (ms) Note: the query logic is actually effective based on second level
endTime	false	integer	The end timestamp (ms) Note: the query logic is actually effective based on second level
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> transferId	string	Transfer ID
> coin	string	Transferred coin
> amount	string	Transferred amount
> fromMemberId	string	From UID
> toMemberId	string	TO UID
> fromAccountType	string	From account type
> toAccountType	string	To account type
> timestamp	string	Transfer created timestamp (ms)
> status	string	Transfer status
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/transfer/query-universal-transfer-list?limit=1&cursor=eyJtaW5JRCI6MTc5NjU3OCwibWF4SUQiOjE3OTY1Nzh9 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN-TYPE: 2
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672190762800
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "list": [
            {
                "transferId": "universalTransfer_4c3cfe2f-85cb-11ed-ac09-9e37823c81cd_533285",
                "coin": "USDC",
                "amount": "1000",
                "timestamp": "1672134373000",
                "status": "SUCCESS",
                "fromAccountType": "UNIFIED",
                "toAccountType": "UNIFIED",
                "fromMemberId": "533285",
                "toMemberId": "592324"
            }
        ],
        "nextPageCursor": "eyJtaW5JRCI6MTc4OTYwNSwibWF4SUQiOjE3ODk2MDV9"
    },
    "retExtInfo": {},
    "time": 1672190763079
}

Set Deposit Account
Set auto transfer account after deposit. The same function as the setting for Deposit on web GUI

info
Your funds will be deposited into FUND wallet by default. You can set the wallet for auto-transfer after deposit by this API.
Only main UID can access.
tip
UTA2.0 has FUND, UNIFIED
UTA1.0 has FUND, UNIFIED, CONTRACT(for inverse derivatives)
Classic account has FUND, CONTRACT(for inverse derivatives and derivatives), SPOT
HTTP Request
POST /v5/asset/deposit/deposit-to-account

Request Parameters
Parameter	Required	Type	Comments
accountType	true	string	Account type
UNIFIED
SPOT
CONTRACT
FUND
Response Parameters
Parameter	Type	Comments
status	integer	Request result:
1: SUCCESS
0: FAIL
RUN >>
Request Example
HTTP
 
  
POST /v5/asset/deposit/deposit-to-account HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676887913670
X-BAPI-RECV-WINDOW: 50000
Content-Type: application/json

{
    "accountType": "CONTRACT"
}

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "status": 1
    },
    "retExtInfo": {},
    "time": 1676887914363
}

Get Deposit Records (on-chain)
Query deposit records

tip
endTime - startTime should be less than 30 days. Query last 30 days records by default.
Support using main or sub UID api key to query deposit records respectively.
HTTP Request
GET /v5/asset/deposit/query-record

Request Parameters
Parameter	Required	Type	Comments
id	false	string	Internal ID: Can be used to uniquely identify and filter the deposit. When combined with other parameters, this field takes the highest priority
txID	false	string	Transaction ID: Please note that data generated before Jan 1, 2024 cannot be queried using txID
coin	false	string	Coin, uppercase only
startTime	false	integer	The start timestamp (ms) Note: the query logic is actually effective based on second level
endTime	false	integer	The end timestamp (ms) Note: the query logic is actually effective based on second level
limit	false	integer	Limit for data size per page. [1, 50]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
rows	array	Object
> coin	string	Coin
> chain	string	Chain
> amount	string	Amount
> txID	string	Transaction ID
> status	integer	Deposit status
> toAddress	string	Deposit target address
> tag	string	Tag of deposit target address
> depositFee	string	Deposit fee
> successAt	string	Last updated time
> confirmations	string	Number of confirmation blocks
> txIndex	string	Transaction sequence number
> blockHash	string	Hash number on the chain
> batchReleaseLimit	string	The deposit limit for this coin in this chain. "-1" means no limit
> depositType	string	The deposit type. 0: normal deposit, 10: the deposit reaches daily deposit limit, 20: abnormal deposit
> fromAddress	string	From address of deposit, only shown when the deposit comes from on-chain and from address is unique, otherwise gives ""
> taxDepositRecordsId	string	This field is used for tax purposes by Bybit EU (Austria) users， declare tax id
> taxStatus	integer	This field is used for tax purposes by Bybit EU (Austria) users
0: No reporting required
1: Reporting pending
2: Reporting completed
> id	string	Unique ID
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/deposit/query-record?coin=USDT&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672191991544
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "rows": [
            {
                "coin": "USDT",
                "chain": "TRX",
                "amount": "999.0496",
                "txID": "04bf3fbad2fc85b107a42cfdc5ff83110092b606ca754efa0f032f8b94b3262e",
                "status": 3,
                "toAddress": "TDGYpm5zPacnEqKV34TJPuhJhHom9hcXAy",
                "tag": "",
                "depositFee": "",
                "successAt": "1742728163000",
                "confirmations": "50",
                "txIndex": "0",
                "blockHash": "000000000436ab4dabc8a4a87beb2262d2d87f6761a825494c4f1d5ae11b27e8",
                "batchReleaseLimit": "-1",
                "depositType": "0",
                "fromAddress": "TJ7hhYhVhaxNx6BPyq7yFpqZrQULL3JSdb",
                "taxDepositRecordsId": "0",
                "taxStatus": 0,
                "id": "160237231"
            }
        ],
        "nextPageCursor": "eyJtaW5JRCI6MTYwMjM3MjMxLCJtYXhJRCI6MTYwMjM3MjMxfQ=="
    },
    "retExtInfo": {},
    "time": 1750798211884
}

Get Sub Deposit Records (on-chain)
Query subaccount's deposit records by main UID's API key.

tip
endTime - startTime should be less than 30 days. Queries for the last 30 days worth of records by default.

HTTP Request
GET /v5/asset/deposit/query-sub-member-record

Request Parameters
Parameter	Required	Type	Comments
id	false	string	Internal ID: Can be used to uniquely identify and filter the deposit. When combined with other parameters, this field takes the highest priority
txID	false	string	Transaction ID: Please note that data generated before Jan 1, 2024 cannot be queried using txID
subMemberId	true	string	Sub UID
coin	false	string	Coin, uppercase only
startTime	false	integer	The start timestamp (ms) Note: the query logic is actually effective based on second level
endTime	false	integer	The end timestamp (ms) Note: the query logic is actually effective based on second level
limit	false	integer	Limit for data size per page. [1, 50]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
rows	array	Object
> id	string	Unique ID
> coin	string	Coin
> chain	string	Chain
> amount	string	Amount
> txID	string	Transaction ID
> status	integer	Deposit status
> toAddress	string	Deposit target address
> tag	string	Tag of deposit target address
> depositFee	string	Deposit fee
> successAt	string	Last updated time
> confirmations	string	Number of confirmation blocks
> txIndex	string	Transaction sequence number
> blockHash	string	Hash number on the chain
> batchReleaseLimit	string	The deposit limit for this coin in this chain. "-1" means no limit
> depositType	string	The deposit type. 0: normal deposit, 10: the deposit reaches daily deposit limit, 20: abnormal deposit
> fromAddress	string	From address of deposit, only shown when the deposit comes from on-chain and from address is unique, otherwise gives ""
> taxDepositRecordsId	string	This field is used for tax purposes by Bybit EU (Austria) users， declare tax id
> taxStatus	integer	This field is used for tax purposes by Bybit EU (Austria) users
0: No reporting required
1: Reporting pending
2: Reporting completed
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/deposit/query-sub-member-record?coin=USDT&limit=1&subMemberId=592334 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672192441294
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "rows": [],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1672192441742
}

Get Internal Deposit Records (off-chain)
Query deposit records within the Bybit platform. These transactions are not on the blockchain.

Rules
The maximum difference between the start time and the end time is 30 days.
Support to get deposit records by Master or Sub Member Api Key
HTTP Request
GET /v5/asset/deposit/query-internal-record

Request Parameters
Parameter	Required	Type	Comments
txID	false	string	Internal transfer transaction ID
startTime	false	integer	Start time (ms). Default value: 30 days before the current time
endTime	false	integer	End time (ms). Default value: current time
coin	false	string	Coin name: for example, BTC. Default value: all
cursor	false	string	Cursor, used for pagination
limit	false	integer	Number of items per page, [1, 50]. Default value: 50
Response Parameters
Parameter	Type	Comments
rows	array	Object
> id	string	ID
> type	integer	1: Internal deposit
> coin	string	Deposit coin
> amount	string	Deposit amount
> status	integer	
1=Processing
2=Success
3=deposit failed
> address	string	Email address or phone number
> createdTime	string	Deposit created timestamp
> txID	string	Internal transfer transaction ID
> taxDepositRecordsId	string	This field is used for tax purposes by Bybit EU (Austria) users， declare tax id
> taxStatus	integer	This field is used for tax purposes by Bybit EU (Austria) users
0: No reporting required
1: Reporting pending
2: Reporting completed
nextPageCursor	string	cursor information: used for pagination. Default value: ""
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/deposit/query-internal-record HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1682099024473
X-BAPI-RECV-WINDOW: 50000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "rows": [
            {
                "id": "1103",
                "amount": "0.1",
                "type": 1,
                "coin": "ETH",
                "address": "xxxx***@gmail.com",
                "status": 2,
                "createdTime": "1705393280",
                "txID": "77c37e5c-d9fa-41e5-bd13-c9b59d95"，
                "taxDepositRecordsId": "0",
                "taxStatus": 0,
            }
        ],
        "nextPageCursor": "eyJtaW5JRCI6MTEwMywibWF4SUQiOjExMDN9"
    },
    "retExtInfo": {},
    "time": 1705395632689
}

Get Master Deposit Address
Query the deposit address information of MASTER account.

HTTP Request
GET /v5/asset/deposit/query-address

Request Parameters
Parameter	Required	Type	Comments
coin	true	string	Coin, uppercase only
chainType	false	string	Please use the value of >> chain from coin-info endpoint
Response Parameters
Parameter	Type	Comments
coin	string	Coin
chains	array	Object
> chainType	string	Chain type
> addressDeposit	string	The address for deposit
> tagDeposit	string	Tag of deposit
> chain	string	Chain
> batchReleaseLimit	string	The deposit limit for this coin in this chain. "-1" means no limit
> contractAddress	string	The contract address of the coin. Only display last 6 characters, if there is no contract address, it shows ""
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/deposit/query-address?coin=USDT&chainType=ETH HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672192792371
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "coin": "USDT",
        "chains": [
            {
                "chainType": "Ethereum (ERC20)",
                "addressDeposit": "XXXXXX",
                "tagDeposit": "",
                "chain": "ETH",
                "batchReleaseLimit": "-1",
                "contractAddress": "831ec7"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1736394811459
}

Get Sub Deposit Address
Query the deposit address information of SUB account.

info
Use master UID's api key only
Custodial sub account deposit address cannot be obtained
HTTP Request
GET /v5/asset/deposit/query-sub-member-address

Request Parameters
Parameter	Required	Type	Comments
coin	true	string	Coin, uppercase only
chainType	true	string	Please use the value of chain from coin-info endpoint
subMemberId	true	string	Sub user ID
Response Parameters
Parameter	Type	Comments
coin	string	Coin
chains	array	Object
> chainType	string	Chain type
> addressDeposit	string	The address for deposit
> tagDeposit	string	Tag of deposit
> chain	string	Chain
> batchReleaseLimit	string	The deposit limit for this coin in this chain. "-1" means no limit
> contractAddress	string	The contract address of the coin. Only display last 6 characters, if there is no contract address, it shows ""
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/deposit/query-sub-member-address?coin=USDT&chainType=TRX&subMemberId=592334 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672194349421
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "coin": "USDT",
        "chains": {
            "chainType": "TRC20",
            "addressDeposit": "XXXXXX",
            "tagDeposit": "",
            "chain": "TRX",
            "batchReleaseLimit": "-1",
            "contractAddress": "gjLj6t"
        }
    },
    "retExtInfo": {},
    "time": 1736394845821
}

Get Withdrawal Records
Query withdrawal records.

tip
endTime - startTime should be less than 30 days. Query last 30 days records by default.
Can query by the master UID's api key only
HTTP Request
GET /v5/asset/withdraw/query-record

Request Parameters
Parameter	Required	Type	Comments
withdrawID	false	string	Withdraw ID
txID	false	string	Transaction hash ID
coin	false	string	Coin, uppercase only
withdrawType	false	integer	Withdraw type. 0(default): on chain. 1: off chain. 2: all
startTime	false	integer	The start timestamp (ms)
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 50]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
rows	array	Object
> txID	string	Transaction ID. It returns "" when withdrawal failed, withdrawal cancelled
> coin	string	Coin
> chain	string	Chain
> amount	string	Amount
> withdrawFee	string	Withdraw fee
> status	string	Withdraw status
> toAddress	string	To withdrawal address. Shows the Bybit UID for internal transfers
> tag	string	Tag
> createTime	string	Withdraw created timestamp (ms)
> updateTime	string	Withdraw updated timestamp (ms)
> withdrawId	string	Withdraw ID
> withdrawType	integer	Withdraw type. 0: on chain. 1: off chain
> fee	string	
> tax	string	
> taxRate	string	
> taxType	string	
nextPageCursor	string	Cursor. Used for pagination
RUN >>
Request Example
HTTP
 
  
GET /v5/asset/withdraw/query-record?coin=USDT&withdrawType=2&limit=2 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672194949557
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "rows": [
            {
                "coin": "USDC",
                "chain": "ETH",
                "amount": "41.43008",
                "txID": "0x3d7bddb797f0e86420c982c0723653b8b728fd0ec9953b6b354445848d83a185",
                "status": "success",
                "toAddress": "0xE3De6d711e0951d34777b5Cd93c827F822ee8514",
                "tag": "",
                "withdrawFee": "5",
                "createTime": "1742738305000",
                "updateTime": "1742738340000",
                "withdrawId": "131629076",
                "withdrawType": 0,
                "fee": "",
                "tax": "",
                "taxRate": "",
                "taxType": ""
            },
            {
                "coin": "USDT",
                "chain": "SOL",
                "amount": "951",
                "txID": "53j7mUftUboJ2TVb1q3zjwNi9gNGWyQ8xhEpkFovzqaTf8LzuZKzr83XjbG62TZWBkWbn27km7SD6Sc9e1BuWUfJ",
                "status": "success",
                "toAddress": "DhTEGye1vq2PPr8DPWit4HTDprnvnDiqpVHnHSY1Y82p",
                "tag": "",
                "withdrawFee": "1",
                "createTime": "1742729329000",
                "updateTime": "1742729437000",
                "withdrawId": "131603458",
                "withdrawType": 0,
                "fee": "",
                "tax": "",
                "taxRate": "",
                "taxType": ""
            }
        ],
        "nextPageCursor": "eyJtaW5JRCI6MTMxNjAzNDU4LCJtYXhJRCI6MTMxNjI5MDc2fQ=="
    },
    "retExtInfo": {},
    "time": 1750777316807
}

Get Exchange Entity List
This endpoint is particularly used for kyc=KOR users. When withdraw funds, you need to fill entity id.

HTTP Request
GET /v5/asset/withdraw/vasp/list

Request Parameters
None

Response Parameters
Parameter	Type	Comments
vasp	array	Exchange entity info
> vaspEntityId	string	Receiver platform id. When transfer to Upbit or other exchanges that not in the list, please use vaspEntityId='others'
> vaspName	string	Receiver platform name
Request Example
HTTP
 
  
GET /v5/asset/withdraw/vasp/list HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1715067106163
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "vasp": [
            {
                "vaspEntityId": "basic-finance",
                "vaspName": "Basic-finance"
            },
            {
                "vaspEntityId": "beeblock",
                "vaspName": "Beeblock"
            },
            {
                "vaspEntityId": "bithumb",
                "vaspName": "bithumb"
            },
            {
                "vaspEntityId": "cardo",
                "vaspName": "cardo"
            },
            {
                "vaspEntityId": "codevasp",
                "vaspName": "codevasp"
            },
            {
                "vaspEntityId": "codexchange-kor",
                "vaspName": "CODExchange-kor"
            },
            {
                "vaspEntityId": "coinone",
                "vaspName": "coinone"
            },
            {
                "vaspEntityId": "dummy",
                "vaspName": "Dummy"
            },
            {
                "vaspEntityId": "flata-exchange",
                "vaspName": "flataexchange"
            },
            {
                "vaspEntityId": "fobl",
                "vaspName": "Foblgate"
            },
            {
                "vaspEntityId": "hanbitco",
                "vaspName": "hanbitco"
            },
            {
                "vaspEntityId": "hexlant",
                "vaspName": "hexlant"
            },
            {
                "vaspEntityId": "inex",
                "vaspName": "INEX"
            },
            {
                "vaspEntityId": "infiniteblock-corp",
                "vaspName": "InfiniteBlock Corp"
            },
            {
                "vaspEntityId": "kdac",
                "vaspName": "kdac"
            },
            {
                "vaspEntityId": "korbit",
                "vaspName": "korbit"
            },
            {
                "vaspEntityId": "paycoin",
                "vaspName": "Paycoin"
            },
            {
                "vaspEntityId": "qbit",
                "vaspName": "Qbit"
            },
            {
                "vaspEntityId": "tennten",
                "vaspName": "TENNTEN"
            },
            {
                "vaspEntityId": "others",
                "vaspName": "Others (including Upbit)"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1715067106537
}

Withdraw
Withdraw assets from your Bybit account. You can make an off-chain transfer if the target wallet address is from Bybit. This means that no blockchain fee will be charged.

tip
Make sure you have whitelisted your wallet address here
Request by the master UID's api key only
formula
feeType=0:

withdrawPercentageFee != 0: handlingFee = inputAmount / (1 - withdrawPercentageFee) * withdrawPercentageFee + withdrawFee
withdrawPercentageFee = 0: handlingFee = withdrawFee
feeType=1:

withdrawPercentageFee != 0: handlingFee = withdrawFee + (inputAmount - withdrawFee) * withdrawPercentageFee
withdrawPercentageFee = 0: handlingFee = withdrawFee
HTTP Request
POST /v5/asset/withdraw/create

Request Parameters
Parameter	Required	Type	Comments
coin	true	string	Coin, uppercase only
chain	false	string	Chain
forceChain=0 or 1: this field is required
forceChain=2: this field can be null
address	true	string	
forceChain=0 or 1: fill wallet address, and make sure you add address in the address book first. Please note that the address is case sensitive, so use the exact same address added in address book
forceChain=2: fill Bybit UID, and it can only be another Bybit main account UID. Make sure you add UID in the address book first
tag	false	string	Tag
Required if tag exists in the wallet address list.
Note: please do not set a tag/memo in the address book if the chain does not support tag
amount	true	string	Withdraw amount
timestamp	true	integer	Current timestamp (ms). Used for preventing from withdraw replay
forceChain	false	integer	Whether or not to force an on-chain withdrawal
0(default): If the address is parsed out to be an internal address, then internal transfer (Bybit main account only)
1: Force the withdrawal to occur on-chain
2: Use UID to withdraw
accountType	true	string	Select the wallet to be withdrawn from
FUND: Funding wallet
UTA: System transfers the funds to Funding wallet to withdraw
FUND,UTA: For combo withdrawals, funds will be deducted from the Funding wallet first. If the balance is insufficient, the remaining amount will be deducted from the UTA wallet.
SPOT: withdraw from spot wallet (classic account only)
feeType	false	integer	Handling fee option
0(default): input amount is the actual amount received, so you have to calculate handling fee manually
1: input amount is not the actual amount you received, the system will help to deduct the handling fee automatically
requestId	false	string	Customised ID, globally unique, it is used for idempotent verification
A combination of letters (case sensitive) and numbers, which can be pure letters or pure numbers and the length must be between 1 and 32 digits
beneficiary	false	Object	Travel rule info. It is required for kyc/kyb=KOR (Korean), kyc=IND (India) users, and users who registered in Bybit Turkey(TR), Bybit Kazakhstan(KZ), Bybit Indonesia (ID)
> vaspEntityId	false	string	Receiver exchange entity Id. Please call this endpoint to get this ID.
Required param for Korean users
Ignored by TR, KZ users
> beneficiaryName	false	string	Receiver exchange user KYC name
Rules for Korean users:
Please refer to target exchange kyc name
When vaspEntityId="others", this field can be null
Rules for TR, KZ, kyc=IND users: it is a required param, fill with individual name or company name
> beneficiaryLegalType	false	string	Beneficiary legal type, individual(default), company
Required param for TR, KZ, kyc=IND users
Korean users can ignore
> beneficiaryWalletType	false	string	Beneficiary wallet type, 0: custodial/exchange wallet (default), 1: non custodial/exchane wallet
Required param for TR, KZ, kyc=IND users
Korean users can ignore
> beneficiaryUnhostedWalletType	false	string	Beneficiary unhosted wallet type, 0: Your own wallet, 1: others' wallet
Required param for TR, KZ, kyc=IND users when "beneficiaryWalletType=1"
Korean users can ignore
> beneficiaryPoiNumber	false	string	Beneficiary ducument number
Required param for TR, KZ users
Korean users can ignore
> beneficiaryPoiType	false	string	Beneficiary ducument type
Required param for TR, KZ users: ID card, Passport, driver license, residence permit, Business ID, etc
Korean users can ignore
> beneficiaryPoiIssuingCountry	false	string	Beneficiary ducument issuing country
Required param for TR, KZ users: refer to Alpha-3 country code
Korean users can ignore
> beneficiaryPoiExpiredDate	false	string	Beneficiary ducument expiry date
Required param for TR, KZ users: yyyy-mm-dd format, e.g., "1990-02-15"
Korean users can ignore
> beneficiaryAddressCountry	false	string	Beneficiary country
Required param for UAE users only, e.g.,IDN
> beneficiaryAddressState	false	string	Beneficiary state
Required param for UAE users only, e.g., "ABC"
> beneficiaryAddressCity	false	string	Beneficiary city
Required param for UAE users only, e.g., "Jakarta"
> beneficiaryAddressBuilding	false	string	Beneficiary building address
Required param for UAE users only
> beneficiaryAddressStreet	false	string	Beneficiary street address
Required param for UAE users only
> beneficiaryAddressPostalCode	false	string	Beneficiary address post code
Required param for UAE users only
> beneficiaryDateOfBirth	false	string	Beneficiary date of birth
Required param for UAE users only
> beneficiaryPlaceOfBirth	false	string	Beneficiary birth place
Required param for UAE users onl
Response Parameters
Parameter	Type	Comments
id	string	Withdrawal ID
Request Example
HTTP
 
  
POST /v5/asset/withdraw/create HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672196570254
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
Content-Type: application/json

{
    "coin": "USDT",
    "chain": "ETH",
    "address": "0x99ced129603abc771c0dabe935c326ff6c86645d",
    "amount": "24",
    "timestamp": 1672196561407,
    "forceChain": 0,
    "accountType": "FUND"
}

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "id": "10195"
    },
    "retExtInfo": {},
    "time": 1672196571239
}

Cancel Withdrawal
Cancel the withdrawal

HTTP Request
POST /v5/asset/withdraw/cancel

Request Parameters
Parameter	Required	Type	Comments
id	true	string	Withdrawal ID
Response Parameters
Parameter	Type	Comments
status	integer	0: fail. 1: success
Request Example
HTTP
 
  
POST /v5/asset/withdraw/cancel HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672197227732
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
Content-Type: application/json

{
    "id": "10197"
}

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "status": 1
    },
    "retExtInfo": {},
    "time": 1672197228408
}

Convert Guideline
info
All convert API endpoints need authentication
API key permission: "Exchange"
Workflow
Step 1: Get Convert Coin List
Query the supported coin list of convert to/from in the different account types.
The balance is also given when querying the convert from coin list.
Step 2: Request a Quote
Select fromCoin, toCoin, acccountType, define the qty of fromCoin to get a quote
There is balance pre-check at this stage.
Step 3: Confirm a Quote
Confirm your quote in the valid time slot (15 secs). Once confirmed, the system processes your transactions.
This operation is async, so it can be failed if you have funds transferred out. Please check the transaction result by step 4.
Step 4: Get Convert Status
Check the final status of the coin convert.

Error
Code	Msg	Comment
32024	exceeds exchange threshold	If the real-time exchange rate (comfirm quote) and quoted rate (apply quote) differ by more than 0.5%, your convert will be rejected/cancelled
790000	system error, please try again later	
700000	parameter error	
700001	quote fail: no deler can be used	
700002	quote fial: not support quote type	when requestCoin=toCoin during request quote stage
700003	order status not allowed	
700004	order does not exit	1. check if quoteTxId is correct; 2. check if quoteTxId is matched with accountType
700005	Your available balance is insufficient or wallet does not exist	
700006	Low amount limit	the request amount cannot be smaller than minFromCoinLimit
700007	Large amount limit	the request amount cannot be larger than maxFromCoinLimit
700008	quote fail: price time out	1. the quote is expired; 2. The quoteTxId does not exist
700009	quoteTxId has already been used	get this error when you call confirm quote more than once before expiry time
700010	INS loan user cannot perform conversion	
700011	illegal operation	when request a quote with user A, but confirm the quote with user B
API Rate Limit
Method	Path	Limit	Upgradable
GET	/v5/asset/exchange/query-coin-list	100 req/s	N
POST	/v5/asset/exchange/quote-apply	50 req/s	N
POST	/v5/asset/exchange/convert-execute	50 req/s	N
GET	/v5/asset/exchange/convert-result-query	100 req/s	N
GET	/v5/asset/exchange/query-convert-history	100 req/s	N

Get Convert Coin List
Query for the list of coins you can convert to/from.

HTTP Request
GET /v5/asset/exchange/query-coin-list

Request Parameters
Parameter	Required	Type	Comments
accountType	true	string	Wallet type
eb_convert_funding
eb_convert_uta
eb_convert_spot
eb_convert_contract
eb_convert_inverse
coin	false	string	Coin, uppercase only
Convert from coin (coin to sell)
when side=0, coin field is ignored
side	false	integer	0: fromCoin list, the balance is given if you have it; 1: toCoin list (coin to buy)
when side=1 and coin field is filled, it returns toCoin list based on coin field
Response Parameters
Parameter	Type	Comments
coins	array<object>	Coin spec
> coin	string	Coin
> fullName	string	Full coin name
> icon	string	Coin icon url
> iconNight	string	Coin icon url (dark mode)
> accuracyLength	integer	Coin precision
> coinType	string	crypto
> balance	string	Balance
When side=0, it gives available balance but cannot used to convert. To get an exact balance to convert, you need specify side=1 and coin parameter
> uBalance	string	Coin balance in USDT worth value
> singleFromMinLimit	string	The minimum amount of fromCoin per transaction
> singleFromMaxLimit	string	The maximum amount of fromCoin per transaction
> disableFrom	boolean	true: the coin is disabled to be fromCoin, false: the coin is allowed to be fromCoin
> disableTo	boolean	true: the coin is disabled to be toCoin, false: the coin is allowed to be toCoin
> timePeriod	integer	Reserved field, ignored for now
> singleToMinLimit	string	Reserved field, ignored for now
> singleToMaxLimit	string	Reserved field, ignored for now
> dailyFromMinLimit	string	Reserved field, ignored for now
> dailyFromMaxLimit	string	Reserved field, ignored for now
> dailyToMinLimit	string	Reserved field, ignored for now
> dailyToMaxLimit	string	Reserved field, ignored for now
Request Example
HTTP
 
  
GET /v5/asset/exchange/query-coin-list?side=0&accountType=eb_convert_funding HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1720064061248
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "coins": [
            {
                "coin": "BTC",
                "fullName": "BTC",
                "icon": "https://t1.bycsi.com/app/assets/token/0717b8c28c2373bf714c964195411d0f.svg",
                "iconNight": "https://t1.bycsi.com/app/assets/token/9504b4c841194cc38f04041003ffbfdb.svg",
                "accuracyLength": 8,
                "coinType": "crypto",
                "balance": "0",
                "uBalance": "0",
                "timePeriod": 0,
                "singleFromMinLimit": "0.001",
                "singleFromMaxLimit": "1",
                "singleToMinLimit": "0",
                "singleToMaxLimit": "0",
                "dailyFromMinLimit": "0",
                "dailyFromMaxLimit": "0",
                "dailyToMinLimit": "0",
                "dailyToMaxLimit": "0",
                "disableFrom": false,
                "disableTo": false
            },
            ...
            {
                "coin": "SOL",
                "fullName": "SOL",
                "icon": "https://s1.bycsi.com/app/assets/token/87ca5f1ca7229bdf0d9a16435653007c.svg",
                "iconNight": "https://t1.bycsi.com/app/assets/token/383a834046655ffe5ef1be1a025791cc.svg",
                "accuracyLength": 8,
                "coinType": "crypto",
                "balance": "18.05988133",
                "uBalance": "2458.46990211775033220586588327",
                "timePeriod": 0,
                "singleFromMinLimit": "0.1",
                "singleFromMaxLimit": "1250",
                "singleToMinLimit": "0",
                "singleToMaxLimit": "0",
                "dailyFromMinLimit": "0",
                "dailyFromMaxLimit": "0",
                "dailyToMinLimit": "0",
                "dailyToMaxLimit": "0",
                "disableFrom": false,
                "disableTo": false
            },
            ...
            {
                "coin": "ETH",
                "fullName": "ETH",
                "icon": "https://s1.bycsi.com/app/assets/token/d6c17c9e767e1810875c702d86ac9f32.svg",
                "iconNight": "https://t1.bycsi.com/app/assets/token/9613ac8e7d62081f4ca20488ae5b168d.svg",
                "accuracyLength": 8,
                "coinType": "crypto",
                "balance": "0.80264489",
                "uBalance": "2596.09751650032773106431534138",
                "timePeriod": 0,
                "singleFromMinLimit": "0.01",
                "singleFromMaxLimit": "250",
                "singleToMinLimit": "0",
                "singleToMaxLimit": "0",
                "dailyFromMinLimit": "0",
                "dailyFromMaxLimit": "0",
                "dailyToMinLimit": "0",
                "dailyToMaxLimit": "0",
                "disableFrom": false,
                "disableTo": false
            }
        ]
    },
    "retExtInfo": {},
    "time": 1720064061736
}

Request a Quote
HTTP Request
POST /v5/asset/exchange/quote-apply

Request Parameters
Parameter	Required	Type	Comments
accountType	true	string	Wallet type
fromCoin	true	string	Convert from coin (coin to sell)
toCoin	true	string	Convert to coin (coin to buy)
requestCoin	true	string	Request coin, same as fromCoin
In the future, we may support requestCoin=toCoin
requestAmount	true	string	request coin amount (the amount you want to sell)
fromCoinType	false	string	crypto
toCoinType	false	string	crypto
paramType	false	string	opFrom, mainly used for API broker user
paramValue	false	string	Broker ID, mainly used for API broker user
requestId	false	string	Customised request ID
a maximum length of 36
Generally it is useless, but it is convenient to track the quote request internally if you fill this field
Response Parameters
Parameter	Type	Comments
quoteTxId	string	Quote transaction ID. It is system generated, and it is used to confirm quote and query the result of transaction
exchangeRate	string	Exchange rate
fromCoin	string	From coin
fromCoinType	string	From coin type. crypto
toCoin	string	To coin
toCoinType	string	To coin type. crypto
fromAmount	string	From coin amount (amount to sell)
toAmount	string	To coin amount (amount to buy according to exchange rate)
expiredTime	string	The expiry time for this quote (15 seconds)
requestId	string	Customised request ID
extTaxAndFee	array	Compliance-related field. Currently returns an empty array, which may be used in the future
Request Example
HTTP
 
  
POST /v5/asset/exchange/quote-apply HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1720071077014
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
Content-Type: application/json
Content-Length: 172

{
    "requestId": "test-00002",
    "fromCoin": "ETH",
    "toCoin": "BTC",
    "accountType": "eb_convert_funding",
    "requestCoin": "ETH",
    "requestAmount": "0.1",
    "paramType": "opFrom",
    "paramValue": "broker-id-001"
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "quoteTxId": "10100108106409340067234418688",
        "exchangeRate": "0.053517914861880000",
        "fromCoin": "ETH",
        "fromCoinType": "crypto",
        "toCoin": "BTC",
        "toCoinType": "crypto",
        "fromAmount": "0.1",
        "toAmount": "0.005351791486188000",
        "expiredTime": "1720071092225",
        "requestId": "test-00002",
        "extTaxAndFee":[]
    },
    "retExtInfo": {},
    "time": 1720071077265
}

Confirm a Quote
info
The exchange is async; please check the final status by calling the query result API.
Make sure you confirm the quote before it expires.
HTTP Request
POST /v5/asset/exchange/convert-execute

Request Parameters
Parameter	Required	Type	Comments
quoteTxId	true	string	The quote tx ID from Request a Quote
Response Parameters
Parameter	Type	Comments
quoteTxId	string	Quote transaction ID
exchangeStatus	string	Exchange status
init
processing
success
failure
Request Example
HTTP
 
  
POST /v5/asset/exchange/convert-execute HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1720071899789
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 52

{
    "quoteTxId": "10100108106409343501030232064"
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "exchangeStatus": "processing",
        "quoteTxId": "10100108106409343501030232064"
    },
    "retExtInfo": {},
    "time": 1720071900529
}

Get Convert Status
You can query the exchange result by sending quoteTxId.

HTTP Request
GET /v5/asset/exchange/convert-result-query

Request Parameters
Parameter	Required	Type	Comments
quoteTxId	true	string	Quote tx ID
accountType	true	string	Wallet type
Response Parameters
Parameter	Type	Comments
result	object	
> accountType	string	Wallet type
> exchangeTxId	string	Exchange tx ID, same as quote tx ID
> userId	string	User ID
> fromCoin	string	From coin
> fromCoinType	string	From coin type. crypto
> toCoin	string	To coin
> toCoinType	string	To coin type. crypto
> fromAmount	string	From coin amount (amount to sell)
> toAmount	string	To coin amount (amount to buy according to exchange rate)
> exchangeStatus	string	Exchange status
init
processing
success
failure
> extInfo	object	Reserved field, ignored for now
> convertRate	string	Exchange rate
> createdAt	string	Quote created time
Request Example
HTTP
 
  
GET /v5/asset/exchange/convert-result-query?quoteTxId=10100108106409343501030232064&accountType=eb_convert_funding HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1720073659847
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "result": {
            "accountType": "eb_convert_funding",
            "exchangeTxId": "10100108106409343501030232064",
            "userId": "XXXXX",
            "fromCoin": "ETH",
            "fromCoinType": "crypto",
            "fromAmount": "0.1",
            "toCoin": "BTC",
            "toCoinType": "crypto",
            "toAmount": "0.00534882723991",
            "exchangeStatus": "success",
            "extInfo": {},
            "convertRate": "0.0534882723991",
            "createdAt": "1720071899995"
        }
    },
    "retExtInfo": {},
    "time": 1720073660696
}

Get Convert History
Returns all confirmed quotes.

info
Only displays the conversion history of conversions created through the API.

HTTP Request
GET /v5/asset/exchange/query-convert-history

Request Parameters
Parameter	Required	Type	Comments
accountType	false	string	Wallet type
Supports passing multiple types, separated by comma e.g., eb_convert_funding,eb_convert_uta
Return all wallet types data if not passed
index	false	integer	Page number
started from 1
1st page by default
limit	false	integer	Page size
20 records by default
up to 100 records, return 100 when exceeds 100
Response Parameters
Parameter	Type	Comments
list	array<object>	Array of quotes
> accountType	string	Wallet type
> exchangeTxId	string	Exchange tx ID, same as quote tx ID
> userId	string	User ID
> fromCoin	string	From coin
> fromCoinType	string	From coin type. crypto
> toCoin	string	To coin
> toCoinType	string	To coin type. crypto
> fromAmount	string	From coin amount (amount to sell)
> toAmount	string	To coin amount (amount to buy according to exchange rate)
> exchangeStatus	string	Exchange status
init
processing
success
failure
> extInfo	object	
>> paramType	string	This field is published when you send it in the Request a Quote
>> paramValue	string	This field is published when you send it in the Request a Quote
> convertRate	string	Exchange rate
> createdAt	string	Quote created time
Request Example
HTTP
 
  
GET /v5/asset/exchange/query-convert-history?accountType=eb_convert_uta,eb_convert_funding HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1720074159814
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "accountType": "eb_convert_funding",
                "exchangeTxId": "10100108106409343501030232064",
                "userId": "XXXXX",
                "fromCoin": "ETH",
                "fromCoinType": "crypto",
                "fromAmount": "0.1",
                "toCoin": "BTC",
                "toCoinType": "crypto",
                "toAmount": "0.00534882723991",
                "exchangeStatus": "success",
                "extInfo": {
                    "paramType": "opFrom",
                    "paramValue": "broker-id-001"
                },
                "convertRate": "0.0534882723991",
                "createdAt": "1720071899995"
            },
            {
                "accountType": "eb_convert_uta",
                "exchangeTxId": "23070eb_convert_uta408933875189391360",
                "userId": "XXXXX",
                "fromCoin": "BTC",
                "fromCoinType": "crypto",
                "fromAmount": "0.1",
                "toCoin": "ETH",
                "toCoinType": "crypto",
                "toAmount": "1.773938248611074",
                "exchangeStatus": "success",
                "extInfo": {},
                "convertRate": "17.73938248611074",
                "createdAt": "1719974243256"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1720074457715
}

Create Sub UID
Create a new sub user id. Use master account's api key.

tip
The API key must have one of the below permissions in order to call this endpoint

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"
HTTP Request
POST /v5/user/create-sub-member

Request Parameters
Parameter	Required	Type	Comments
username	true	string	Give a username of the new sub user id.
6-16 characters, must include both numbers and letters.
cannot be the same as the exist or deleted one.
password	false	string	Set the password for the new sub user id.
8-30 characters, must include numbers, upper and lowercase letters.
memberType	true	integer	1: normal sub account, 6: custodial sub account
switch	false	integer	
0: turn off quick login (default)
1: turn on quick login.
isUta	false	boolean	deprecated param, always UTA account
note	false	string	Set a remark
Response Parameters
Parameter	Type	Comments
uid	string	Sub user Id
username	string	Give a username of the new sub user id.
6-16 characters, must include both numbers and letters.
cannot be the same as the exist or deleted one.
memberType	integer	1: normal sub account, 6: custodial sub account
status	integer	The status of the user account
1: normal
2: login banned
4: frozen
remark	string	The remark
Request Example
HTTP
 
  
POST /v5/user/create-sub-member HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676429344202
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "username": "xxxxx",
    "memberType": 1,
    "switch": 1,
    "note": "test"
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "uid": "53888000",
        "username": "xxxxx",
        "memberType": 1,
        "status": 1,
        "remark": "test"
    },
    "retExtInfo": {},
    "time": 1676429344734
}

Create Sub UID API Key
To create new API key for those newly created sub UID. Use master user's api key only.

tip
The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"
HTTP Request
POST /v5/user/create-sub-api

Request Parameters
Parameter	Required	Type	Comments
subuid	true	integer	Sub user Id
note	false	string	Set a remark
readOnly	true	integer	0：Read and Write. 1：Read only
ips	false	string	Set the IP bind. example: "192.168.0.1,192.168.0.2"note:
don't pass ips or pass with "*" means no bind
No ip bound api key will be invalid after 90 days
api key without IP bound will be invalid after 7 days once the account password is changed
permissions	true	Object	Tick the types of permission.
one of below types must be passed, otherwise the error is thrown
> ContractTrade	false	array	Contract Trade. ["Order","Position"]
> Spot	false	array	Spot Trade. ["SpotTrade"]
> Options	false	array	USDC Contract. ["OptionsTrade"]
> Wallet	false	array	Wallet. ["AccountTransfer","SubMemberTransferList"]
Note: Fund Custodial account is not supported
> Exchange	false	array	Convert. ["ExchangeHistory"]
> Earn	false	array	Earn product. ["Earn"]
> Derivatives	false	array	This param is deprecated because system will automatically add this permission according to your account is UTA or Classic
> CopyTrading	false	array	Copytrade. ["CopyTrading"] deprecated because using signal copy trading now, please refer to How To Start Copy Trading
Response Parameters
Parameter	Type	Comments
id	string	Unique id. Internal used
note	string	The remark
apiKey	string	Api key
readOnly	integer	0: Read and Write. 1: Read only
secret	string	The secret paired with api key.
The secret can't be queried by GET api. Please keep it properly
permissions	Object	The types of permission
> ContractTrade	array	Permisson of contract trade
> Spot	array	Permisson of spot
> Wallet	array	Permisson of wallet
> Options	array	Permission of USDC Contract. It supports trade option and usdc perpetual.
> Derivatives	array	Permission of Unified account
> Exchange	array	Permission of convert
> Earn	array	Permission of earn product
> CopyTrading	array	Permission of copytrade, deprecated always []
> BlockTrade	array	Permission of blocktrade. Not applicable to sub account, always []
> NFT	array	Deprecated, always []
Request Example
HTTP
 
  
POST /v5/user/create-sub-api HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676430005459
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "subuid": 53888000,
    "note": "testxxx",
    "readOnly": 0,
    "permissions": {
        "Wallet": [
            "AccountTransfer"
        ]
    }
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "id": "16651283",
        "note": "testxxx",
        "apiKey": "xxxxx",
        "readOnly": 0,
        "secret": "xxxxxxxx",
        "permissions": {
            "ContractTrade": [],
            "Spot": [],
            "Wallet": [
                "AccountTransfer"
            ],
            "Options": [],
            "CopyTrading": [],
            "BlockTrade": [],
            "Exchange": [],
            "NFT": [],
            "Earn": ["Earn"]
        }
    },
    "retExtInfo": {},
    "time": 1676430007643
}

Get Sub UID List (Limited)
Get at most 10k sub UID of master account. Use master user's api key only.

tip
The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"
HTTP Request
GET /v5/user/query-sub-members

Request Parameters
None

Response Parameters
Parameter	Type	Comments
subMembers	array	Object
> uid	string	Sub user Id
> username	string	Username
> memberType	integer	1: normal sub account, 6: custodial sub account
> status	integer	The status of the user account
1: normal
2: login banned
4: frozen
> accountMode	integer	The account mode of the user account
1: classic account
3: UTA
> remark	string	The remark
Request Example
HTTP
 
  
GET /v5/user/query-sub-members HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676430318405
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "subMembers": [
            {
                "uid": "53888001",
                "username": "xxx001",
                "memberType": 1,
                "status": 1,
                "remark": "test",
                "accountMode": 3
            },
            {
                "uid": "53888002",
                "username": "xxx002",
                "memberType": 6,
                "status": 1,
                "remark": "",
                "accountMode": 1
            }
        ]
    },
    "retExtInfo": {},
    "time": 1676430319452
}

Get Sub UID List (Unlimited)
This API is applicable to the client who has over 10k sub accounts. Use master user's api key only.

tip
The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"
HTTP Request
GET /v5/user/submembers

Request Parameters
Parameter	Required	Type	Comments
pageSize	false	string	Data size per page. Return up to 100 records per request
nextCursor	false	string	Cursor. Use the nextCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
subMembers	array	Object
> uid	string	Sub user Id
> username	string	Username
> memberType	integer	1: standard sub account, 6: custodial sub account
> status	integer	The status of the user account
1: normal
2: login banned
4: frozen
> accountMode	integer	The account mode of the user account
1: Classic Account
3: Unified Trading Account
> remark	string	The remark
nextCursor	string	The next page cursor value. "0" means no more pages
Request Example
HTTP
 
GET /v5/user/submembers?pageSize=1 HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676430318405
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "subMembers": [
            {
                "uid": "100475023",
                "username": "BybitmcYERjAPmMU",
                "memberType": 1,
                "status": 1,
                "remark": "",
                "accountMode": 1
            }
        ],
        "nextCursor": "126671"
    },
    "retExtInfo": {},
    "time": 1711695552772
}

Get Fund Custodial Sub Acct
The institutional client can query the fund custodial sub accounts.

tip
The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"
HTTP Request
GET /v5/user/escrow_sub_members

Request Parameters
Parameter	Required	Type	Comments
pageSize	false	string	Data size per page. Return up to 100 records per request
nextCursor	false	string	Cursor. Use the nextCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
subMembers	array	Object
> uid	string	子帳戶userId
> username	string	用戶名
> memberType	integer	12: 基金託管子帳戶
> status	integer	帳戶狀態.
1: 正常
2: 登陸封禁
4: 凍結
> accountMode	integer	帳戶模式.
1: 經典帳戶
3: UTA帳戶
> remark	string	備註
nextCursor	string	下一頁數據的游標. 返回"0"表示沒有更多的數據了
Request Example
HTTP
 
  
GET /v5/user/escrow_sub_members?pageSize=2 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1739763787703
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "subMembers": [
            {
                "uid": "104274894",
                "username": "Private_Wealth_Management",
                "memberType": 12,
                "status": 1,
                "remark": "earn fund",
                "accountMode": 3
            },
            {
                "uid": "104274884",
                "username": "Private_Wealth_Management",
                "memberType": 12,
                "status": 1,
                "remark": "earn fund",
                "accountMode": 3
            }
        ],
        "nextCursor": "344"
    },
    "retExtInfo": {},
    "time": 1739763788699
}

Freeze Sub UID
Freeze Sub UID. Use master user's api key only.

tip
The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"
HTTP Request
POST /v5/user/frozen-sub-member

Request Parameters
Parameter	Required	Type	Comments
subuid	true	integer	Sub user Id
frozen	true	integer	0：unfreeze, 1：freeze
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/user/frozen-sub-member HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676430842094
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "subuid": 53888001,
    "frozen": 1
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {},
    "retExtInfo": {},
    "time": 1676430697553
}

Get API Key Information
Get the information of the api key. Use the api key pending to be checked to call the endpoint. Both master and sub user's api key are applicable.

tip
Any permission can access this endpoint.

HTTP Request
GET /v5/user/query-api

Request Parameters
None

Response Parameters
Parameter	Type	Comments
id	string	Unique ID. Internal use
note	string	The remark
apiKey	string	Api key
readOnly	integer	0：Read and Write. 1：Read only
secret	string	Always ""
permissions	Object	The types of permission
> ContractTrade	array	Permission of contract trade Order, Position
> Spot	array	Permission of spot SpotTrade
> Wallet	array	Permission of wallet AccountTransfer, SubMemberTransfer(master account), SubMemberTransferList(sub account), Withdraw(master account)
> Options	array	Permission of USDC Contract. It supports trade option and USDC perpetual. OptionsTrade
> Derivatives	array	
Unified account has this permission by default DerivativesTrade
For classic account, it is always []
> Exchange	array	Permission of convert ExchangeHistory
> Earn	array	Permission of earn product Earn
> NFT	array	Deprecated, always []
> BlockTrade	array	Permission of blocktrade. Not applicable to subaccount, always []
> Affiliate	array	Permission of Affiliate. Only affiliate can have this permission, otherwise always []
> CopyTrading	array	Always [] as Master Trader account just use ContractTrade to start CopyTrading
ips	array	IP bound
type	integer	The type of api key. 1：personal, 2：connected to the third-party app
deadlineDay	integer	The remaining valid days of api key. Only for those api key with no IP bound or the password has been changed
expiredAt	datetime	The expiry day of the api key. Only for those api key with no IP bound or the password has been changed
createdAt	datetime	The create day of the api key
unified	integer	deprecated field
uta	integer	Whether the account to which the account upgrade to unified trade account. 0：regular account; 1：unified trade account
userID	integer	User ID
inviterID	integer	Inviter ID (the UID of the account which invited this account to the platform)
vipLevel	string	VIP Level
mktMakerLevel	string	Market maker level
affiliateID	integer	Affiliate Id. 0 represents that there is no binding relationship.
rsaPublicKey	string	Rsa public key
isMaster	boolean	If this api key belongs to master account or not
parentUid	string	The main account uid. Returns "0" when the endpoint is called by main account
kycLevel	string	Personal account kyc level. LEVEL_DEFAULT, LEVEL_1， LEVEL_2
kycRegion	string	Personal account kyc region
RUN >>
Request Example
HTTP
 
  
GET /v5/user/query-api HTTP/1.1
Host: api.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676430842094
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "id": "13770661",
        "note": "readwrite api key",
        "apiKey": "XXXXXX",
        "readOnly": 0,
        "secret": "",
        "permissions": {
            "ContractTrade": [
                "Order",
                "Position"
            ],
            "Spot": [
                "SpotTrade"
            ],
            "Wallet": [
                "AccountTransfer",
                "SubMemberTransfer"
            ],
            "Options": [
                "OptionsTrade"
            ],
            "Derivatives": [],
            "CopyTrading": [],
            "BlockTrade": [],
            "Exchange": [],
            "NFT": [],
            "Affiliate": [],
            "Earn": []
        },
        "ips": [
            "*"
        ],
        "type": 1,
        "deadlineDay": 66,
        "expiredAt": "2023-12-22T07:20:25Z",
        "createdAt": "2022-10-16T02:24:40Z",
        "unified": 0,
        "uta": 0,
        "userID": 24617703,
        "inviterID": 0,
        "vipLevel": "No VIP",
        "mktMakerLevel": "0",
        "affiliateID": 0,
        "rsaPublicKey": "",
        "isMaster": true,
        "parentUid": "0",
        "kycLevel": "LEVEL_DEFAULT",
        "kycRegion": ""
    },
    "retExtInfo": {},
    "time": 1697525990798
}

Get Sub Account All API Keys
Query all api keys information of a sub UID.

tip
Any permission can access this endpoint
Only master account can call this endpoint
HTTP Request
GET /v5/user/sub-apikeys

Request Parameters
Parameter	Required	Type	Comments
subMemberId	true	string	Sub UID
limit	false	integer	Limit for data size per page. [1, 20]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
result	array	Object
> id	string	Unique ID. Internal use
> ips	array<string>	IP bound
> apiKey	string	Api key
> note	string	The remark
> status	integer	1: permanent, 2: expired, 3: within the validity period, 4: expires soon (less than 7 days)
> expiredAt	datetime	The expiry day of the api key. Only for those api key with no IP bound or the password has been changed
> createdAt	datetime	The create day of the api key
> type	integer	The type of api key. 1: personal, 2: connected to the third-party app
> permissions	Object	The types of permission
>> ContractTrade	array	Permission of contract trade Order, Position
>> Spot	array	Permission of spot SpotTrade
>> Wallet	array	Permission of wallet AccountTransfer, SubMemberTransferList
>> Options	array	Permission of USDC Contract. It supports trade option and USDC perpetual. OptionsTrade
>> Derivatives	array	Unified account api key have this permission by default. DerivativesTrade
>> Exchange	array	Permission of convert ExchangeHistory
>> Earn	array	Permission of earn product Earn
>> CopyTrading	array	Always [], Master Trader uses "Contract" permission to start Copytrading
>> BlockTrade	array	Permission of blocktrade. Not applicable to subaccount, always []
>> NFT	array	Deprecated, always []
>> Affiliate	array	Permission of Affiliate. Not applicable to sub account, always []
> secret	string	Always "******"
> readOnly	boolean	true, false
> deadlineDay	integer	The remaining valid days of api key. Only for those api key with no IP bound or the password has been changed
> flag	string	Api key type
nextPageCursor	string	Refer to the cursor request parameter
RUN >>
Request Example
HTTP
 
  
GET /v5/user/sub-apikeys?subMemberId=100400345 HTTP/1.1
Host: api.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1699515251088
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "result": [
            {
                "id": "24828209",
                "ips": [
                    "*"
                ],
                "apiKey": "XXXXXX",
                "note": "UTA",
                "status": 3,
                "expiredAt": "2023-12-01T02:36:06Z",
                "createdAt": "2023-08-25T06:42:39Z",
                "type": 1,
                "permissions": {
                    "ContractTrade": [
                        "Order",
                        "Position"
                    ],
                    "Spot": [
                        "SpotTrade"
                    ],
                    "Wallet": [
                        "AccountTransfer",
                        "SubMemberTransferList"
                    ],
                    "Options": [
                        "OptionsTrade"
                    ],
                    "Derivatives": [
                        "DerivativesTrade"
                    ],
                    "CopyTrading": [],
                    "BlockTrade": [],
                    "Exchange": [
                        "ExchangeHistory"
                    ],
                    "NFT": [],
                    "Affiliate": [],
                    "Earn": []
                },
                "secret": "******",
                "readOnly": false,
                "deadlineDay": 21,
                "flag": "hmac"
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1699515251698
}

Get UID Wallet Type
Get available wallet types for the master account or sub account

tip
Master api key: you can get master account and appointed sub account available wallet types, and support up to 200 sub UID in one request.
Sub api key: you can get its own available wallet types
PRACTICE
"FUND" - If you never deposit or transfer capital into it, this wallet type will not be shown in the array, but your account indeed has this wallet.

["SPOT","FUND","CONTRACT"] : Classic account and Funding wallet was operated before
["SPOT","CONTRACT"] : Classic account and Funding wallet is never operated
["UNIFIED""FUND","CONTRACT"] : UTA account and Funding wallet was operated before.
["UNIFIED","CONTRACT"] : UTA account and Funding wallet is never operated.
HTTP Request
GET /v5/user/get-member-type

Request Parameters
Parameter	Required	Type	Comments
memberIds	false	string	
Query itself wallet types when not passed
When use master api key to query sub UID, master UID data is always returned in the top of the array
Multiple sub UID are supported, separated by commas
This param is ignored when you use sub account api key
Response Parameters
Parameter	Type	Comments
accounts	array	Object
> uid	string	Master/Sub user Id
> accountType	array	Wallets array. SPOT, CONTRACT, FUND, OPTION, UNIFIED. Please check above practice to understand the value
RUN >>
Request Example
HTTP
 
  
GET /v5/user/get-member-type HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1686884973961
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "accounts": [
            {
                "uid": "533285",
                "accountType": [
                    "SPOT",
                    "UNIFIED",
                    "FUND",
                    "CONTRACT"
                ]
            }
        ]
    },
    "retExtInfo": {},
    "time": 1686884974151
}

Modify Master API Key
Modify the settings of master api key. Use the api key pending to be modified to call the endpoint. Use master user's api key only.

tip
The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"
info
Only the api key that calls this interface can be modified

HTTP Request
POST /v5/user/update-api

Request Parameters
Parameter	Required	Type	Comments
readOnly	false	integer	0 (default)：Read and Write. 1：Read only
ips	false	string	Set the IP bind. example: "192.168.0.1,192.168.0.2"note:
don't pass ips or pass with "*" means no bind
No ip bound api key will be invalid after 90 days
api key will be invalid after 7 days once the account password is changed
permissions	false	Object	Tick the types of permission. Don't send this param if you don't want to change the permission
> ContractTrade	false	array	Contract Trade. ["Order","Position"]
> Spot	false	array	Spot Trade. ["SpotTrade"]
> Wallet	false	array	Wallet. ["AccountTransfer","SubMemberTransfer"]
> Options	false	array	USDC Contract. ["OptionsTrade"]
> Derivatives	false	array	This param is deprecated because system will automatically add this permission according to your account is UTA or Classic
> Exchange	false	array	Convert. ["ExchangeHistory"]
> Earn	false	array	Earn product. ["Earn"]
> CopyTrading	false	array	Copytrade. ["CopyTrading"], deprecated
> BlockTrade	false	array	Blocktrade. ["BlockTrade"]
> NFT	false	array	Deprecated
> Affiliate	false	array	Affiliate. ["Affiliate"]
This permission is only useful for affiliate
If you need this permission, make sure you remove all other permissions
Response Parameters
Parameter	Type	Comments
id	string	Unique id. Internal used
note	string	The remark
apiKey	string	Api key
readOnly	integer	0：Read and Write. 1：Read only
secret	string	Always ""
permissions	Object	The types of permission
> ContractTrade	array	Permisson of contract trade
> Spot	array	Permisson of spot
> Wallet	array	Permisson of wallet
> Options	array	Permission of USDC Contract. It supports trade option and usdc perpetual.
> Derivatives	array	Permission of Unified account
> CopyTrading	array	Permission of copytrade. Not applicable to sub account, always []
> BlockTrade	array	Permission of blocktrade. Not applicable to sub account, always []
> Exchange	array	Permission of convert
> Earn	array	Permission of Earn
> NFT	array	Deprecated, always []
ips	array	IP bound
Request Example
HTTP
 
  
POST /v5/user/update-api HTTP/1.1
Host: api.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676431264739
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
Content-Type: application/json

{
    "readOnly": null,
    "ips": "*",
    "permissions": {
            "ContractTrade": [
                "Order",
                "Position"
            ],
            "Spot": [
                "SpotTrade"
            ],
            "Wallet": [
                "AccountTransfer",
                "SubMemberTransfer"
            ],
            "Options": [
                "OptionsTrade"
            ],
            "CopyTrading": [
                "CopyTrading"
            ],
            "BlockTrade": [],
            "Exchange": [
                "ExchangeHistory"
            ],
            "NFT": [
                "NFTQueryProductList"
            ]
        }
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "id": "13770661",
        "note": "xxxxx",
        "apiKey": "xxxxx",
        "readOnly": 0,
        "secret": "",
        "permissions": {
            "ContractTrade": [
                "Order",
                "Position"
            ],
            "Spot": [
                "SpotTrade"
            ],
            "Wallet": [
                "AccountTransfer",
                "SubMemberTransfer"
            ],
            "Options": [
                "OptionsTrade"
            ],
            "Derivatives": [
                "DerivativesTrade"
            ],
            "CopyTrading": [
                "CopyTrading"
            ],
            "BlockTrade": [],
            "Exchange": [
                "ExchangeHistory"
            ],
            "Earn": [],
            "NFT": [
                "NFTQueryProductList"
            ]
        },
        "ips": [
            "*"
        ]
    },
    "retExtInfo": {},
    "time": 1676431265427
}

Modify Sub API Key
Modify the settings of sub api key. Use the sub account api key pending to be modified to call the endpoint or use master account api key to manage its sub account api key.

tip
The API key must have one of the below permissions in order to call this endpoint

sub API key: "Account Transfer", "Sub Member Transfer"
master API Key: "Account Transfer", "Sub Member Transfer", "Withdrawal"
HTTP Request
POST /v5/user/update-sub-api

Request Parameters
Parameter	Required	Type	Comments
apikey	false	string	Sub account api key
You must pass this param when you use master account manage sub account api key settings
If you use corresponding sub uid api key call this endpoint, apikey param cannot be passed, otherwise throwing an error
readOnly	false	integer	0 (default)：Read and Write. 1：Read only
ips	false	string	Set the IP bind. example: "192.168.0.1,192.168.0.2"note:
don't pass ips or pass with "*" means no bind
No ip bound api key will be invalid after 90 days
api key will be invalid after 7 days once the account password is changed
permissions	false	Object	Tick the types of permission. Don't send this param if you don't want to change the permission
> ContractTrade	false	array	Contract Trade. ["Order","Position"]
> Spot	false	array	Spot Trade. ["SpotTrade"]
> Wallet	false	array	Wallet. ["AccountTransfer", "SubMemberTransferList"]
Note: fund custodial account is not supported
> Options	false	array	USDC Contract. ["OptionsTrade"]
> Derivatives	false	array	This param is deprecated because system will automatically add this permission according to your account is UTA or Classic
> Exchange	false	array	Convert. ["ExchangeHistory"]
> Earn	false	array	Earn product. ["Earn"]
> CopyTrading	false	array	Copytrade. ["CopyTrading"]
Response Parameters
Parameter	Type	Comments
id	string	Unique id. Internal used
note	string	The remark
apiKey	string	Api key
readOnly	integer	0：Read and Write. 1：Read only
secret	string	Always ""
permissions	Object	The types of permission
> ContractTrade	array	Permisson of contract trade
> Spot	array	Permisson of spot
> Wallet	array	Permisson of wallet
> Options	array	Permission of USDC Contract. It supports trade option and usdc perpetual.
> Derivatives	array	Permission of Unified account
> CopyTrading	array	Permission of copytrade
> BlockTrade	array	Permission of blocktrade. Not applicable to sub account, always []
> Exchange	array	Permission of convert
> Earn	array	Permission of Earn
> NFT	array	Deprecated, always []
ips	array	IP bound
Request Example
HTTP
 
  
POST /v5/user/update-sub-api HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676431795752
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "readOnly": 0,
    "ips": "*",
    "permissions": {
            "ContractTrade": [],
            "Spot": [
                "SpotTrade"
            ],
            "Wallet": [
                "AccountTransfer"
            ],
            "Options": [],
            "CopyTrading": [],
            "BlockTrade": [],
            "Exchange": [],
            "NFT": []
        }
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "id": "16651472",
        "note": "testxxx",
        "apiKey": "xxxxxx",
        "readOnly": 0,
        "secret": "",
        "permissions": {
            "ContractTrade": [],
            "Spot": [
                "SpotTrade"
            ],
            "Wallet": [
                "AccountTransfer"
            ],
            "Options": [],
            "Derivatives": [],
            "CopyTrading": [],
            "BlockTrade": [],
            "Exchange": [],
            "NFT": []
        },
        "ips": [
            "*"
        ]
    },
    "retExtInfo": {},
    "time": 1676431796263
}

Delete Sub UID
Delete a sub UID. Before deleting the UID, please make sure there is no asset.
Use master user's api key**.

tip
The API key must have one of the below permissions in order to call this endpoint

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"
HTTP Request
POST /v5/user/del-submember

Request Parameters
Parameter	Required	Type	Comments
subMemberId	true	string	Sub UID
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/user/del-submember HTTP/1.1
Host: api.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1698907012755
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
Content-Type: application/json
Content-Length: 34

{
    "subMemberId": "112725187"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtInfo": {},
    "time": 1698907012962
}

Delete Master API Key
Delete the api key of master account. Use the api key pending to be delete to call the endpoint. Use master user's api key only.

tip
The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"
danger
BE CAREFUL! The API key used to call this interface will be invalid immediately.

HTTP Request
POST /v5/user/delete-api

Request Parameters
None

Response Parameters
None

Request Example
HTTP
 
  
POST /v5/user/delete-api HTTP/1.1
Host: api.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676431576621
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
Content-Type: application/json

{

}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {},
    "retExtInfo": {},
    "time": 1676431577675
}

Delete Sub API Key
Delete the api key of sub account. Use the sub api key pending to be delete to call the endpoint or use the master api key to delete corresponding sub account api key

tip
The API key must have one of the below permissions in order to call this endpoint.

sub API key: "Account Transfer", "Sub Member Transfer"
master API Key: "Account Transfer", "Sub Member Transfer", "Withdrawal"
danger
BE CAREFUL! The Sub account API key will be invalid immediately after calling the endpoint.

HTTP Request
POST /v5/user/delete-sub-api

Request Parameters
Parameter	Required	Type	Comments
apikey	false	string	Sub account api key
You must pass this param when you use master account manage sub account api key settings
If you use corresponding sub uid api key call this endpoint, apikey param cannot be passed, otherwise throwing an error
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/user/delete-sub-api HTTP/1.1
Host: api.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676431922953
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
Content-Type: application/json

{

}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {},
    "retExtInfo": {},
    "time": 1676431924719
}

Get Instruments Info
Query for the instrument specification of spread combinations.

HTTP Request
GET /v5/spread/instrument

Request Parameters
Parameter	Required	Type	Comments
symbol	false	string	Spread combination symbol name
baseCoin	false	string	Base coin, uppercase only
limit	false	integer	Limit for data size per page. [1, 500]. Default: 200
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array<object>	instrument info
> symbol	string	Spread combination symbol name
> contractType	string	Product type
FundingRateArb: perpetual & spot combination
CarryTrade: futures & spot combination
FutureSpread: different expiry futures combination
PerpBasis: futures & perpetual
> status	string	Spread status. Trading, Settling
> baseCoin	string	Base coin
> quoteCoin	string	Quote coin
> settleCoin	string	Settle coin
> tickSize	string	The step to increase/reduce order price
> minPrice	string	Min. order price
> maxPrice	string	Max. order price
> lotSize	string	Order qty precision
> minSize	string	Min. order qty
> maxSize	string	Max. order qty
> launchTime	string	Launch timestamp (ms)
> deliveryTime	string	Delivery timestamp (ms)
> legs	array<object>	Legs information
>> symbol	string	Legs symbol name
>> contractType	string	Legs contract type. LinearPerpetual, LinearFutures, Spot
nextPageCursor	string	Refer to the cursor request parameter
Request Example
GET /v5/spread/instrument?limit=1 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "symbol": "SOLUSDT_SOL/USDT",
                "contractType": "FundingRateArb",
                "status": "Trading",
                "baseCoin": "SOL",
                "quoteCoin": "USDT",
                "settleCoin": "USDT",
                "tickSize": "0.0001",
                "minPrice": "-1999.9998",
                "maxPrice": "1999.9998",
                "lotSize": "0.1",
                "minSize": "0.1",
                "maxSize": "50000",
                "launchTime": "1743675300000",
                "deliveryTime": "0",
                "legs": [
                    {
                        "symbol": "SOLUSDT",
                        "contractType": "LinearPerpetual"
                    },
                    {
                        "symbol": "SOLUSDT",
                        "contractType": "Spot"
                    }
                ]
            }
        ],
        "nextPageCursor": "first%3D100008%26last%3D100008"
    },
    "retExtInfo": {},
    "time": 1744076802479
}

Get Orderbook
Query spread orderbook depth data.

HTTP Request
GET /v5/spread/orderbook

Request Parameters
Parameter	Required	Type	Comments
symbol	true	string	Spread combination symbol name
limit	false	integer	Limit size for each bid and ask [1, 25]. Default: 1
Response Parameters
Parameter	Type	Comments
s	string	Spread combination symbol name
b	array	Bid, buyer. Sorted by price in descending order
> b[0]	string	Bid price
> b[1]	string	Bid size
a	array	Ask, seller. Sorted by price in ascending order
> a[0]	string	Ask price
> a[1]	string	Ask size
ts	integer	The timestamp (ms) that the system generates the data
u	integer	Update ID. Is always in sequence. Corresponds to u in the 25-level WebSocket orderbook stream
seq	integer	Cross sequence
cts	integer	The timestamp from the matching engine when this orderbook data is produced. It can be correlated with T from public trade channel
Request Example
GET /v5/spread/orderbook?symbol=SOLUSDT_SOL/USDT&limit=1 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "Success",
    "result": {
        "s": "SOLUSDT_SOL/USDT",
        "b": [
            [
                "21.0000",
                "0.1"
            ]
        ],
        "a": [
            [
                "23.0107",
                "4.6"
            ]
        ],
        "u": 46977,
        "ts": 1744077242177,
        "seq": 213110,
        "cts": 1744076329043
    },
    "retExtInfo": {},
    "time": 1744077243583
}

Get Tickers
Query for the latest price snapshot, best bid/ask price, and trading volume of different spread combinations in the last 24 hours.

HTTP Request
GET /v5/spread/tickers

Request Parameters
Parameter	Required	Type	Comments
symbol	true	string	Spread combination symbol name
Response Parameters
Parameter	Type	Comments
list	array<object>	Ticker info
> symbol	string	Spread combination symbol name
> bidPrice	string	Bid 1 price
> bidSize	string	Bid 1 size
> askPrice	string	Ask 1 price
> askSize	string	Ask 1 size
> lastPrice	string	Last trade price
> highPrice24h	string	The highest price in the last 24 hours
> lowPrice24h	string	The lowest price in the last 24 hours
> prevPrice24h	string	Price 24 hours a  
> volume24h	string	Volume for 24h
Request Example
GET /v5/spread/tickers?symbol=SOLUSDT_SOL/USDT HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "Success",
    "result": {
        "list": [
            {
                "symbol": "SOLUSDT_SOL/USDT",
                "bidPrice": "",
                "bidSize": "",
                "askPrice": "",
                "askSize": "",
                "lastPrice": "19.444",
                "highPrice24h": "23.8353",
                "lowPrice24h": "0",
                "prevPrice24h": "20",
                "volume24h": "24694.9"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1744079413254
}

Get Recent Public Trades
Query recent public spread trading history in Bybit.

HTTP Request
GET /v5/spread/recent-trade

Request Parameters
Parameter	Required	Type	Comments
symbol	true	string	Spread combination symbol name
limit	false	integer	Limit for data size per page [1,1000], default: 500
Response Parameters
Parameter	Type	Comments
list	array<object>	Public trade info
> execId	string	Execution ID
> symbol	string	Spread combination symbol name
> price	string	Trade price
> size	string	Trade size
> side	string	Side of taker Buy, Sell
> time	string	Trade time (ms)
> seq	string	Cross sequence
Request Example
GET /v5/spread/recent-trade?symbol=SOLUSDT_SOL/USDT&limit=2 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "Success",
    "result": {
        "list": [
            {
                "execId": "c8512970-d6fb-5039-93a5-b4196dffbe88",
                "symbol": "SOLUSDT_SOL/USDT",
                "price": "20.2805",
                "size": "3.3",
                "side": "Sell",
                "time": "1744078324035",
                "seq":"123456"
            },
            {
                "execId": "92b0002e-c49d-5618-a195-4140d7e10a2b",
                "symbol": "SOLUSDT_SOL/USDT",
                "price": "20.843",
                "size": "2.2",
                "side": "Buy",
                "time": "1744078322010",
                "seq":"123450"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1744078324682
}

Create Order
Place a spread combination order. Up to 50 open orders per account.

HTTP Request
POST /v5/spread/order/create

Request Parameters
Parameter	Required	Type	Comments
symbol	true	string	Spread combination symbol name
side	true	string	Order side. Buy, Sell
orderType	true	string	Limit, Market
qty	true	string	Order qty
price	false	string	Order price
orderLinkId	false	string	User customised order ID, a max of 45 characters. Combinations of numbers, letters (upper and lower cases), dashes, and underscores are supported.
timeInForce	false	string	Time in force. IOC, FOK, GTC, PostOnly
info
The acknowledgement of an place order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

Response Parameters
Parameter	Type	Comments
orderId	string	Spread combination order ID
orderLinkId	string	User customised order ID
Request Example
POST /v5/spread/order/create HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1744079410023
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 191

{
    "symbol": "SOLUSDT_SOL/USDT",
    "side": "Buy",
    "orderType": "Limit",
    "qty": "0.1",
    "price": "21",
    "orderLinkId": "1744072052193428479",
    "timeInForce": "PostOnly"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderId": "1b00b997-d825-465e-ad1d-80b0eb1955af",
        "orderLinkId": "1744072052193428479"
    },
    "retExtInfo": {},
    "time": 1744075839332
}

Amend Order
info
You can only modify unfilled or partially filled orders.

HTTP Request
POST /v5/spread/order/amend

Request Parameters
Parameter	Required	Type	Comments
symbol	true	string	Spread combination symbol name
orderId	false	string	Spread combination order ID. Either orderId or orderLinkId is required
orderLinkId	false	string	User customised order ID. Either orderId or orderLinkId is required
qty	false	string	Order quantity after modification. Either qty or price is required
price	false	string	Order price after modification
Either qty or price is required
price="" means the price remains unchanged, while price="0" updates the price to 0.
info
The acknowledgement of an amend order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

Response Parameters
Parameter	Type	Comments
orderId	string	Order ID
orderLinkId	string	User customised order ID
Request Example
POST /v5/spread/order/amend HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1744083949347
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 115

{
    "symbol": "SOLUSDT_SOL/USDT",
    "orderLinkId": "1744072052193428475",
    "price": "14",
    "qty": "0.2"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderId": "b0e6c938-9731-4122-8552-01e6dc06b303",
        "orderLinkId": "1744072052193428475"
    },
    "retExtInfo": {},
    "time": 1744083952599
}

Cancel Order
HTTP Request
POST /v5/spread/order/cancel

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Spread combination order ID. Either orderId or orderLinkId is required
orderLinkId	false	string	User customised order ID. Either orderId or orderLinkId is required
Response Parameters
Parameter	Type	Comments
orderId	string	Order ID
orderLinkId	string	User customised order ID
info
The acknowledgement of an cancel order request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

Request Example
POST /v5/spread/order/cancel HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: XXXXXXX
X-BAPI-TIMESTAMP: 1744090699418
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 48

{
    "orderLinkId": "1744072052193428476"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "orderId": "4496253b-b55b-4407-8c5c-29629d169caf",
        "orderLinkId": "1744072052193428476"
    },
    "retExtInfo": {},
    "time": 1744090702715
}

Cancel All Orders
Cancel all open orders

HTTP Request
POST /v5/spread/order/cancel-all

Request Parameters
Parameter	Required	Type	Comments
symbol	false	string	Spread combination symbol name
When a symbol is specified, all orders for that symbol will be cancelled regardless of the cancelAll field.
When a symbol is not specified and cancelAll=true, all orders, regardless of the symbol, will be cancelled
cancelAll	false	boolean	true, false
info
The acknowledgement of cancel all orders request indicates that the request was sucessfully accepted. This request is asynchronous so please use the websocket to confirm the order status.

Response Parameters
Parameter	Type	Comments
list	array<object>	
> orderId	string	Order ID
> orderLinkId	string	User customised order ID
success	string	The field can be ignored
Request Example
POST /v5/spread/order/cancel-all HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1744090967121
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 49

{
    "symbol": null,
    "cancelAll": true
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "orderId": "11ec47f3-f0a2-4b2a-b302-236f2a2d53a2",
                "orderLinkId": ""
            }
        ],
        "success": "1"
    },
    "retExtInfo": {},
    "time": 1744090940933
}

Get Open Orders
HTTP Request
GET /v5/spread/order/realtime

Request Parameters
Parameter	Required	Type	Comments
symbol	false	string	Spread combination symbol name
baseCoin	false	string	Base coin
orderId	false	string	Spread combination order ID
orderLinkId	false	string	User customised order ID
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array<object>	Order info
> symbol	string	Spread combination symbol name
> baseCoin	string	Base coin
> orderType	string	Order type, Market, Limit
> orderLinkId	string	User customised order ID
> side	string	Side, Buy, Sell
> timeInForce	string	Time in force, GTC, FOK, IOC, PostOnly
> orderId	string	Spread combination order ID
> leavesQty	string	The remaining qty not executed
> orderStatus	string	Order status, New, PartiallyFilled
> cumExecQty	string	Cumulative executed order qty
> price	string	Order price
> qty	string	Order qty
> createdTime	string	Order created timestamp (ms)
> updatedTime	string	Order updated timestamp (ms)
nextPageCursor	string	Refer to the cursor request parameter
Request Example
GET /v5/spread/order/realtime HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1744096099520
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "nextPageCursor": "aaaee090-fab3-42ea-aea0-c9fbfe6c4bc4%3A1744096099767%2Caaaee090-fab3-42ea-aea0-c9fbfe6c4bc4%3A1744096099767",
        "list": [
            {
                "symbol": "SOLUSDT_SOL/USDT",
                "orderType": "Limit",
                "updatedTime": "1744096099771",
                "orderLinkId": "",
                "side": "Buy",
                "orderId": "aaaee090-fab3-42ea-aea0-c9fbfe6c4bc4",
                "leavesQty": "0.1",
                "orderStatus": "New",
                "cumExecQty": "0",
                "price": "-4",
                "qty": "0.1",
                "createdTime": "1744096099767",
                "timeInForce": "PostOnly",
                "baseCoin": "SOL"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1744096103435
}

Get Order History
info
orderId & orderLinkId has a higher priority than startTime & endTime
Fully cancelled orders are stored for up to 24 hours.
Single leg orders can also be found with "createType"=CreateByFutureSpread via Get Order History

HTTP Request
GET /v5/spread/order/history

Request Parameters
Parameter	Required	Type	Comments
symbol	false	string	Spread combination symbol name
baseCoin	false	string	Base coin
orderId	false	string	Spread combination order ID
orderLinkId	false	string	User customised order ID
startTime	false	long	The start timestamp (ms)
startTime and endTime are not passed, return 7 days by default
Only startTime is passed, return range between startTime and startTime+7 days
Only endTime is passed, return range between endTime-7 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	long	The end timestamp (ms)
limit	false	integer	Limit for data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array<object>	Order info
> symbol	string	Spread combination symbol name
> orderType	string	Order type, Market, Limit
> orderLinkId	string	User customised order ID
> orderId	string	Spread combination order ID
> contractType	string	Combo type
FundingRateArb: perpetual & spot combination
CarryTrade: futures & spot combination
FutureSpread: different expiry futures combination
PerpBasis: futures & perpetual
> cxlRejReason	string	Reject reason
> orderStatus	string	Order status, Rejected, Cancelled, Filled
> price	string	Order price
> orderQty	string	Order qty
> timeInForce	string	Time in force, GTC, FOK, IOC, PostOnly
> baseCoin	string	Base coin
> createdAt	string	Order created timestamp (ms)
> updatedAt	string	Order updated timestamp (ms)
> side	string	Side, Buy, Sell
> leavesQty	string	The remaining qty not executed. It is meaningless for a cancelled order
> settleCoin	string	Settle coin
> cumExecQty	string	Cumulative executed order qty
> qty	string	Order qty
> leg1Symbol	string	Leg1 symbol name
> leg1ProdType	string	Leg1 product type, Futures, Spot
> leg1OrderId	string	Leg1 order ID
> leg1Side	string	Leg1 order side
> leg2ProdType	string	Leg2 product type, Futures, Spot
> leg2OrderId	string	Leg2 order ID
> leg2Symbol	string	Leg2 symbol name
> leg2Side	string	Leg2 orde side
nextPageCursor	string	Refer to the cursor request parameter
Request Example
GET /v5/spread/order/history?orderId=aaaee090-fab3-42ea-aea0-c9fbfe6c4bc4 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1744100522465
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "Success",
    "result": {
        "nextPageCursor": "aaaee090-fab3-42ea-aea0-c9fbfe6c4bc4%3A1744096099767%2Caaaee090-fab3-42ea-aea0-c9fbfe6c4bc4%3A1744096099767",
        "list": [
            {
                "symbol": "SOLUSDT_SOL/USDT",
                "orderType": "Limit",
                "orderLinkId": "",
                "orderId": "aaaee090-fab3-42ea-aea0-c9fbfe6c4bc4",
                "contractType": "FundingRateArb",
                "orderStatus": "Cancelled",
                "createdAt": "1744096099767",
                "price": "-4",
                "leg2Symbol": "SOLUSDT",
                "orderQty": "0.1",
                "timeInForce": "PostOnly",
                "baseCoin": "SOL",
                "updatedAt": "1744098396079",
                "side": "Buy",
                "leg2Side": "Sell",
                "leavesQty": "0",
                "leg1Side": "Buy",
                "settleCoin": "USDT",
                "cumExecQty": "0",
                "qty": "0.1",
                "leg1OrderId": "82335b0a-b7d9-4ea5-9230-e71271a65100",
                "leg2OrderId": "1924011967786517249",
                "leg2ProdType": "Spot",
                "leg1ProdType": "Futures",
                "leg1Symbol": "SOLUSDT"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1744102655725
}

Get Trade History
info
In self-trade cases, both the maker and taker single-leg trades will be returned in the same request.
Single leg executions can also be found with "execType"=FutureSpread via Get Trade History
HTTP Request
GET /v5/spread/execution/list

Request Parameters
Parameter	Required	Type	Comments
symbol	false	string	Spread combination symbol name
orderId	false	string	Spread combination order ID
orderLinkId	false	string	User customised order ID
startTime	false	long	The start timestamp (ms)
startTime and endTime are not passed, return 7 days by default
Only startTime is passed, return range between startTime and startTime+7 days
Only endTime is passed, return range between endTime-7 days and endTime
If both are passed, the rule is endTime - startTime <= 7 days
endTime	false	long	The end timestamp (ms)
limit	false	integer	Limit for parent order data size per page. [1, 50]. Default: 20
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array<object>	Trade info
> symbol	string	Spread combination symbol name
> orderLinkId	string	User customised order ID
> side	string	Side, Buy, Sell
> orderId	string	Spread combination order ID
> execPrice	string	Combo Exec price
> execTime	string	Combo exec timestamp (ms)
> execType	string	Combo exec type, Trade
> execQty	string	Combo exec qty
> execId	string	Combo exec ID
> legs	array<object>	Legs execution info
>> symbol	string	Leg symbol name
>> side	string	Leg order side, Buy, Sell
>> execPrice	string	Leg exec price
>> execTime	string	Leg exec timestamp (ms)
>> execValue	string	Leg exec value
>> execType	string	Leg exec type
>> cate  ry	string	Leg cate  ry, linear, spot
>> execQty	string	Leg exec qty
>> execFee	string	Leg exec fee, deprecated for Spot leg
>> execFeeV2	string	Leg exec fee, used for Spot leg only
>> feeCurrency	string	Leg fee currency
>> execId	string	Leg exec ID
nextPageCursor	string	Refer to the cursor request parameter
Request Example
GET /v5/spread/execution/list?orderId=5e010c35-2b44-4f03-8081-8fa31fb73376 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: XXXXX
X-BAPI-TIMESTAMP: 1744105738529
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "Success",
    "result": {
        "nextPageCursor": "82c82077-0caa-5304-894d-58a50a342bd7%3A1744104992219%2C82c82077-0caa-5304-894d-58a50a342bd7%3A1744104992219",
        "list": [
            {
                "symbol": "SOLUSDT_SOL/USDT",
                "orderLinkId": "",
                "side": "Buy",
                "orderId": "5e010c35-2b44-4f03-8081-8fa31fb73376",
                "execPrice": "21",
                "legs": [
                    {
                        "symbol": "SOLUSDT",
                        "side": "Buy",
                        "execPrice": "124.1",
                        "execTime": "1744104992224",
                        "execValue": "248.2",
                        "execType": "FutureSpread",
                        "cate  ry": "linear",
                        "execQty": "2",
                        "execFee": "0.039712",
                        "execId": "99a18f80-d3b5-4c6f-a1f1-8c5870e3f3bc"
                    },
                    {
                        "symbol": "SOLUSDT",
                        "side": "Sell",
                        "execPrice": "103.1152",
                        "execTime": "1744104992224",
                        "execValue": "206.2304",
                        "execType": "FutureSpread",
                        "cate  ry": "spot",
                        "execQty": "2",
                        "execFee": "0.06186912",
                        "execId": "2110000000061481958"
                    }
                ],
                "execTime": "1744104992220",
                "execType": "Trade",
                "execQty": "2",
                "execId": "82c82077-0caa-5304-894d-58a50a342bd7"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1744105105169
}

Connect
WebSocket public stream:

Mainnet:
Spot: wss://stream.bybit.com/v5/public/spot
USDT, USDC perpetual & USDT Futures: wss://stream.bybit.com/v5/public/linear
Inverse contract: wss://stream.bybit.com/v5/public/inverse
Spread trading: wss://stream.bybit.com/v5/public/spread
USDT/USDC Options: wss://stream.bybit.com/v5/public/option

Testnet:
Spot: wss://stream-testnet.bybit.com/v5/public/spot
USDT,USDC perpetual & USDT Futures: wss://stream-testnet.bybit.com/v5/public/linear
Inverse contract: wss://stream-testnet.bybit.com/v5/public/inverse
Spread trading: wss://stream-testnet.bybit.com/v5/public/spread
USDT/USDC Options: wss://stream-testnet.bybit.com/v5/public/option

WebSocket private stream:

Mainnet:
wss://stream.bybit.com/v5/private

Testnet:
wss://stream-testnet.bybit.com/v5/private

WebSocket Order Entry:

Mainnet:
wss://stream.bybit.com/v5/trade (Spread trading is not supported)

Testnet:
wss://stream-testnet.bybit.com/v5/trade (Spread trading is not supported)

WebSocket GET System Status:

Mainnet:
wss://stream.bybit.com/v5/public/misc/status

Testnet:
wss://stream-testnet.bybit.com/v5/public/misc/status

info
If your account is registered from www.bybit-tr.com, please use stream.bybit-tr.com for mainnet access
If your account is registered from www.bybit.kz, please use stream.bybit.kz for mainnet access
If your account is registered from www.bybitgeorgia.ge, please use stream.bybitgeorgia.ge for mainnet access
Customise Private Connection Alive Time
For private stream and order entry, you can customise alive duration by adding a param max_active_time, the lowest value is 30s (30 seconds), the highest value is 600s (10 minutes). You can also pass 1m, 2m etc when you try to configure by minute level. e.g., wss://stream-testnet.bybit.com/v5/private?max_active_time=1m.

In general, if there is no "ping-pong" and no stream data sent from server end, the connection will be cut off after 10 minutes. When you have a particular need, you can configure connection alive time by max_active_time.

Since ticker scans every 30s, so it is not fully exact, i.e., if you configure 45s, and your last update or ping-pong is occurred on 2023-08-15 17:27:23, your disconnection time maybe happened on 2023-08-15 17:28:15

Authentication
info
Public topics do not require authentication. The following section applies to private topics only.
Apply for authentication when establishing a connection.

Note: if you're using pybit, bybit-api, or another high-level library, you can ignore this code - as authentication is handled for you.

{
    "req_id": "10001", // optional
    "op": "auth",
    "args": [
        "api_key",
        1662350400000, // expires; is greater than your current timestamp
        "signature"
    ]
}

# based on: https://github.com/bybit-exchange/pybit/blob/master/pybit/_http_manager.py

import hmac
import json
import time
import websocket

api_key = ""
api_secret = ""

# Generate expires.
expires = int((time.time() + 1) * 1000)

# Generate signature.
signature = str(hmac.new(
    bytes(api_secret, "utf-8"),
    bytes(f"GET/realtime{expires}", "utf-8"), digestmod="sha256"
).hexdigest())

ws = websocket.WebSocketApp(
    url=url,
    ...
)

# Authenticate with API.
ws.send(
    json.dumps({
        "op": "auth",
        "args": [api_key, expires, signature]
    })
)

Successful authentication sample response

{
    "success": true,
    "ret_msg": "",
    "op": "auth",
    "conn_id": "cejreaspqfh3sjdnldmg-p"
}

note
Example signature al  rithms can be found here.

caution
Due to network complexity, your may get disconnected at any time. Please follow the instructions below to ensure that you receive WebSocket messages on time:

Keep connection alive by sending the heartbeat packet
Reconnect as soon as possible if disconnected
IP Limits
Do not frequently connect and disconnect the connection.
Do not build over 500 connections in 5 minutes. This is counted per WebSocket domain.
Public channel - Args limits
Regardless of Perpetual, Futures, Options or Spot, for one public connection, you cannot have length of "args" array over 21,000 characters.

Spot can input up to 10 args for each subscription request sent to one connection
Options can input up to 2000 args for a single connection
No args limit for Futures and Spread for now
How to Send the Heartbeat Packet
How to Send

// req_id is a customised ID, which is optional
ws.send(JSON.stringify({"req_id": "100001", "op": "ping"}));

Pong message example of public channels

Spot
Linear/Inverse
Option/Spread
{
    "success": true,
    "ret_msg": "pong",
    "conn_id": "0970e817-426e-429a-a679-ff7f55e0b16a",
    "op": "ping"
}

Pong message example of private channels

{
    "req_id": "test",
    "op": "pong",
    "args": [
        "1675418560633"
    ],
    "conn_id": "cfcb4ocsvfriu23r3er0-1b"
}

caution
To avoid network or program issues, we recommend that you send the ping heartbeat packet every 20 seconds to maintain the WebSocket connection.

How to Subscribe to Topics
Understanding WebSocket Filters
How to subscribe with a filter

// Subscribing level 1 orderbook
{
    "req_id": "test", // optional
    "op": "subscribe",
    "args": [
        "orderbook.1.BTCUSDT"
    ]
}

Subscribing with multiple symbols and topics is supported.

{
    "req_id": "test", // optional
    "op": "subscribe",
    "args": [
        "orderbook.1.BTCUSDT",
        "publicTrade.BTCUSDT",
        "orderbook.1.ETHUSDT"
    ]
}

Understanding WebSocket Filters: Unsubscription
You can dynamically subscribe and unsubscribe from topics without unsubscribing from the WebSocket like so:

{
    "op": "unsubscribe",
    "args": [
        "publicTrade.ETHUSD"
    ],
    "req_id": "customised_id"
}

Understanding the Subscription Response
Topic subscription response message example

Private
Public Spot
Linear/Inverse
Option/Spread
{
    "success": true,
    "ret_msg": "",
    "op": "subscribe",
    "conn_id": "cejreassvfrsfvb9v1a0-2m"
}

Orderbook
Subscribe to the spread orderbook stream.

Depths
Level 25 data, push frequency: 20ms

Topic:
orderbook.{depth}.{symbol} e.g., orderbook.25.SOLUSDT_SOL/USDT

Process snapshot/delta
To process snapshot and delta messages, please follow these rules:

Once you have subscribed successfully, you will receive a snapshot. The WebSocket will keep pushing delta messages every time the orderbook changes. If you receive a new snapshot message, you will have to reset your local orderbook. If there is a problem on Bybit's end, a snapshot will be re-sent, which is guaranteed to contain the latest data.

To apply delta updates:

If you receive an amount that is 0, delete the entry
If you receive an amount that does not exist, insert it
If the entry exists, you simply update the value
See working code examples of this logic in the FAQ.

Response Parameters
Parameter	Type	Comments
topic	string	Topic name
type	string	Data type. snapshot,delta
ts	number	The timestamp (ms) that the system generates the data
data	map	Object
> s	string	Symbol name
> b	array	Bids. For snapshot stream. Sorted by price in descending order
>> b[0]	string	Bid price
>> b[1]	string	Bid size
The delta data has size=0, which means that all quotations for this price have been filled or cancelled
> a	array	Asks. For snapshot stream. Sorted by price in ascending order
>> a[0]	string	Ask price
>> a[1]	string	Ask size
The delta data has size=0, which means that all quotations for this price have been filled or cancelled
> u	integer	Update ID
Occasionally, you'll receive "u"=1, which is a snapshot data due to the restart of the service. So please overwrite your local orderbook
> seq	integer	Cross sequence
cts	number	The timestamp from the matching engine when this orderbook data is produced. It can be correlated with T from public trade channel
Subscribe Example
{
    "op": "subscribe",
    "args": ["orderbook.25.SOLUSDT_SOL/USDT"]
}

Event Example
{
    "topic": "orderbook.25.SOLUSDT_SOL/USDT",
    "ts": 1744165512257,
    "type": "delta",
    "data": {
        "s": "SOLUSDT_SOL/USDT",
        "b": [],
        "a": [
            [
                "22.3755",
                "4.7"
            ]
        ],
        "u": 64892,
        "seq": 299084
    },
    "cts": 1744165512234
}

Trade
Subscribe to the public trades stream.

After subscription, you will be pushed trade messages in real-time.

Push frequency: real-time

Topic:
publicTrade.{symbol}

Response Parameters
Parameter	Type	Comments
topic	string	Topic name
type	string	Data type. snapshot
ts	number	The timestamp (ms) that the system generates the data
data	array	Object. Sorted by the time the trade was matched in ascending order
> T	number	The timestamp (ms) that the order is filled
> s	string	Symbol name
> S	string	Side of taker. Buy,Sell
> v	string	Trade size
> p	string	Trade price
> L	string	Direction of price change
> i	string	Trade ID
> seq	integer	Cross sequence
Subscribe Example
{
    "op": "subscribe",
    "id": "test-001-perp",
    "args": [
        "publicTrade.SOLUSDT_SOL/USDT"
    ]
}

Response Example
{
    "topic": "publicTrade.SOLUSDT_SOL/USDT",
    "ts": 1744170142723,
    "type": "snapshot",
    "data": [
        {
            "T": 1744170142720,
            "s": "SOLUSDT_SOL/USDT",
            "S": "Sell",
            "v": "2.5",
            "p": "19.3928",
            "L": "MinusTick",
            "i": "31d0fc58-933b-57b3-8378-f73da06da843",
            "seq": 1783284617
        }
    ]
}

Ticker
Subscribe to the ticker stream.

Push frequency: 100ms

Topic:
tickers.{symbol}

Response Parameters
Parameter	Type	Comments
topic	string	Topic name
type	string	Data type. snapshot
ts	number	The timestamp (ms) that the system generates the data
data	map	Object
> symbol	string	Spread combination symbol name
> bidPrice	string	Bid 1 price
> bidSize	string	Bid 1 size
> askPrice	string	Ask 1 price
> askSize	string	Ask 1 size
> lastPrice	string	Last trade price
> highPrice24h	string	The highest price in the last 24 hours
> lowPrice24h	string	The lowest price in the last 24 hours
> prevPrice24h	string	Price 24 hours a  
> volume24h	string	Volume for 24h
Subscribe Example
{
    "op": "subscribe",
    "args": [
        "tickers.SOLUSDT_SOL/USDT"
    ]
}

Event Example
{
    "topic": "tickers.SOLUSDT_SOL/USDT",
    "ts": 1744168585009,
    "type": "snapshot",
    "data": {
        "symbol": "SOLUSDT_SOL/USDT",
        "bidPrice": "20.3359",
        "bidSize": "1.7",
        "askPrice": "",
        "askSize": "",
        "lastPrice": "21.8182",
        "highPrice24h": "24.2356",
        "lowPrice24h": "-3",
        "prevPrice24h": "22.1468",
        "volume24h": "23309.9"
    }
}

Order
Subscribe to the order stream to see changes to your orders in real-time.

Topic: spread.order

Response Parameters
Parameter	Type	Comments
id	string	Message ID
topic	string	Topic name
creationTime	number	Data created timestamp (ms)
data	array	Object
> cate  ry	string	Cate  ry name, combination, spot_leg, future_leg
> symbol	string	Combo or leg's symbol name
> parentOrderId	string	Leg's parent order ID
> orderId	string	Combo or leg's order ID
> orderLinkId	string	Combo's user customised order ID
> side	string	Combo or leg's order side, Buy, Sell
> orderStatus	string	Combo or leg's order status
> cancelType	string	Cancel type
> rejectReason	string	Reject reason
> timeInForce	string	Time in force, GTC, FOK, IOC, PostOnly
> price	string	Order price
> qty	string	Order qty
> avgPrice	string	Average filled price
> leavesQty	string	The remaining qty not executed
> leavesValue	string	The estimated value not executed
> cumExecQty	string	Cumulative executed order qty
> cumExecValue	string	Cumulative executed order value
> cumExecFee	string	Cumulative executed trading fee
> orderType	string	Order type. Market,Limit
> isLeverage	string	Account-wide, if Spot Margin is enabled, the spot_leg field in the execution message shows 1, combo is "", and future_leg is 0.
> createdTime	string	Order created timestamp (ms)
> updatedTime	string	Order updated timestamp (ms)
> feeCurrency	string	Trading fee currency for Spot leg only
> createType	string	Order create type
> closedPnl	string	Closed profit and loss for each close position order
Subscribe Example
{
    "op": "subscribe",
    "args": [
        "spread.order"
    ]
}

Stream Example
{
    "topic": "spread.order",
    "id": "1448939_SOLUSDT_28732003549",
    "creationTime": 1744170555912,
    "data": [
        {
            "cate  ry": "combination",
            "symbol": "SOLUSDT_SOL/USDT",
            "parentOrderId": "",
            "orderId": "aa858ea9-f3a0-40b6-ad57-888d47307345",
            "orderLinkId": "",
            "side": "Buy",
            "orderStatus": "Filled",
            "cancelType": "UNKNOWN",
            "rejectReason": "EC_NoError",
            "timeInForce": "GTC",
            "price": "14",
            "qty": "2",
            "avgPrice": "",
            "leavesQty": "0",
            "leavesValue": "",
            "cumExecQty": "2",
            "cumExecValue": "",
            "cumExecFee": "",
            "orderType": "Limit",
            "isLeverage": "",
            "createdTime": "1744170534447",
            "updatedTime": "1744170555905",
            "feeCurrency": "",
            "createType": "CreateByUser",
            "closedPnl": ""
        },
        {
            "cate  ry": "future_leg",
            "symbol": "SOLUSDT",
            "parentOrderId": "aa858ea9-f3a0-40b6-ad57-888d47307345",
            "orderId": "2948d2dc-f8f1-4485-a83d-0bad3dae2c31",
            "orderLinkId": "",
            "side": "Buy",
            "orderStatus": "Filled",
            "cancelType": "UNKNOWN",
            "rejectReason": "EC_NoError",
            "timeInForce": "GTC",
            "price": "118.2",
            "qty": "2",
            "avgPrice": "118.2",
            "leavesQty": "0",
            "leavesValue": "0",
            "cumExecQty": "2",
            "cumExecValue": "236.4",
            "cumExecFee": "0.01182",
            "orderType": "Limit",
            "isLeverage": "",
            "createdTime": "1744170534447",
            "updatedTime": "1744170555910",
            "feeCurrency": "",
            "createType": "CreateByFutureSpread",
            "closedPnl": "0"
        }
    ]
}

Execution
Topic: spread.execution

Response Parameters
Parameter	Type	Comments
id	string	Message ID
topic	string	Topic name
creationTime	number	Data created timestamp (ms)
data	array<object>	
> cate  ry	string	Combo or single leg, combination, spot_leg, future_leg
> symbol	string	Combo or leg symbol name
> isLeverage	string	Account-wide, if Spot Margin is enabled, the spot_leg field in the execution message shows 1, combo is "", and future_leg is 0.
> orderId	string	Order ID, leg is ""
> orderLinkId	string	User customized order ID, leg is ""
> side	string	Side. Buy,Sell
> orderPrice	string	Order price
> orderQty	string	Order qty
> leavesQty	string	The remaining qty not executed
> createType	string	Order create type
> orderType	string	Order type. Market,Limit
> execFee	string	Leg exec fee, deprecated for Spot leg
> execFeeV2	string	Leg exec fee, used for Spot leg only
> feeCurrency	string	Leg fee currency
> parentExecId	string	Combo's Execution ID, leg's event has the value
> execId	string	Execution ID
> execPrice	string	Execution price
> execQty	string	Execution qty
> execPnl	string	Profit and Loss for each close position execution
> execType	string	Executed type
> execValue	string	Executed order value
> execTime	string	Executed timestamp (ms)
> isMaker	boolean	Is maker order. true: maker, false: taker
> feeRate	string	Trading fee rate
> markPrice	string	The mark price of the symbol when executing
> closedSize	string	Closed position size
> seq	long	Cross sequence
Subscribe Example
{
    "op": "subscribe",
    "args": [
        "spread.execution"
    ]
}

Stream Example
// Combo execution
{
     "topic": "spread.execution",
     "id": "cvqes8141ilt347i9l20",
     "creationTime": 1744104992226,
     "data": [
          {
               "cate  ry": "combination",
               "symbol": "SOLUSDT_SOL/USDT",
               "closedSize": "",
               "execFee": "",
               "execId": "82c82077-0caa-5304-894d-58a50a342bd7",
               "parentExecId": "",
               "execPrice": "20.9848",
               "execQty": "2",
               "execType": "Trade",
               "execValue": "",
               "feeRate": "",
               "markPrice": "",
               "leavesQty": "0",
               "orderId": "5e010c35-2b44-4f03-8081-8fa31fb73376",
               "orderLinkId": "",
               "orderPrice": "21",
               "orderQty": "2",
               "orderType": "Limit",
               "side": "Buy",
               "execTime": "1744104992220",
               "isLeverage": "",
               "isMaker": false,
               "seq": 241321,
               "createType": "CreateByUser",
               "execPnl": ""
          }
     ]
}

//Future leg execution
{
     "topic": "spread.execution",
     "id": "1448939_SOLUSDT_28731107101",
     "creationTime": 1744104992229,
     "data": [
          {
               "cate  ry": "future_leg",
               "symbol": "SOLUSDT",
               "closedSize": "0",
               "execFee": "0.039712",
               "execId": "99a18f80-d3b5-4c6f-a1f1-8c5870e3f3bc",
               "parentExecId": "82c82077-0caa-5304-894d-58a50a342bd7",
               "execPrice": "124.1",
               "execQty": "2",
               "execType": "FutureSpread",
               "execValue": "248.2",
               "feeRate": "0.00016",
               "markPrice": "119",
               "leavesQty": "0",
               "orderId": "",
               "orderLinkId": "",
               "orderPrice": "124.1",
               "orderQty": "2",
               "orderType": "Limit",
               "side": "Buy",
               "execTime": "1744104992224",
               "isLeverage": "0",
               "isMaker": false,
               "seq": 28731107101,
               "createType": "CreateByFutureSpread",
               "execPnl": "0"
          }
     ]
}

Get Affiliate User List
To use this endpoint, you should have an affiliate account and only tick "affiliate" permission while creating the API key.
Affiliate site: https://affiliates.bybit.com

tip
Use master UID only
The api key can only have "Affiliate" permission
HTTP Request
GET /v5/affiliate/aff-user-list

Request Parameters
Parameter	Required	Type	Comments
size	false	integer	Limit for data size per page. [0, 1000]. Default: 0
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
needDeposit	false	boolean	true: return deposit info; false(default): does not return deposit info
need30	false	boolean	true: return 30 days trading info; false(default): does not return 30 days trading info
need365	false	boolean	true: return 365 days trading info; false(default): does not return 365 days trading info
Response Parameters
Parameter	Type	Comments
list	array	Object
> userId	string	user Id
> registerTime	string	user register time
> source	integer	user registration source, from which referrer code
> remarks	integer	The remark
> isKyc	boolean	Whether KYC is completed
> takerVol30Day	string	Taker volume in last 30 days (USDT), update at T + 1. All volume related attributes below includes Derivatives, Option, Spot volume
> makerVol30Day	string	Maker volume in last 30 days (USDT), update at T + 1
> tradeVol30Day	string	Total trading volume in last 30 days (USDT), update at T + 1
> depositAmount30Day	string	Deposit amount in last 30 days (USDT)
> takerVol365Day	string	Taker volume in the past year (USDT), update at T + 1
> makerVol365Day	string	Maker volume in the past year (USDT), update at T + 1
> tradeVol365Day	string	Total trading volume in the past year (USDT), update at T + 1
> depositAmount365Day	string	Total deposit amount in the past year (USDT)
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/affiliate/aff-user-list?cursor=""&size=2 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1685596324209
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: xxxxxx
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "list": [
            {
                "userId": "101527036",
                "registerTime": "2024-06-21",
                "source": "1564",
                "remarks": "test",
                "isKyc": false,
                "takerVol30Day": "10",
                "makerVol30Day": "20",
                "tradeVol30Day": "30",
                "depositAmount30Day": "90",
                "takerVol365Day": "100",
                "makerVol365Day": "500",
                "tradeVol365Day": "600",
                "depositAmount365Day": "1300",
            },
            {
                "userId": "103929118",
                "registerTime": "2024-11-12",
                "source": "1564",
                "remarks": "",
                "isKyc": false,
                "takerVol30Day": "10",
                "makerVol30Day": "20",
                "tradeVol30Day": "30",
                "depositAmount30Day": "90",
                "takerVol365Day": "100",
                "makerVol365Day": "500",
                "tradeVol365Day": "600",
                "depositAmount365Day": "1300",
            }
        ],
        "nextPageCursor": "16197"
    },
    "retExtInfo": {},
    "time": 1733205472513
}


Get Affiliate User Info
To use this endpoint, you should have an affiliate account and only tick "affiliate" permission while creating the API key.
Affiliate site: https://affiliates.bybit.com

tip
Use master UID only
The api key can only have "Affiliate" permission
The transaction volume and deposit amount are the total amount of the user done on Bybit, and have nothing to do with commission settlement. Any transaction volume data related to commission settlement is subject to the Affiliate Portal.
HTTP Request
GET /v5/user/aff-customer-info

Request Parameters
Parameter	Required	Type	Comments
uid	true	string	The master account UID of affiliate's client
Response Parameters
Parameter	Type	Comments
uid	string	UID
vipLevel	string	VIP level
takerVol30Day	string	Taker volume in last 30 days (USDT). All volume related attributes below includes Derivatives, Option, Spot volume
makerVol30Day	string	Maker volume in last 30 days (USDT)
tradeVol30Day	string	Total trading volume in last 30 days (USDT)
depositAmount30Day	string	Deposit amount in last 30 days (USDT), update in 5 mins
takerVol365Day	string	Taker volume in the past year (USDT)
makerVol365Day	string	Maker volume in the past year (USDT)
tradeVol365Day	string	Total trading volume in the past year (USDT)
depositAmount365Day	string	Total deposit amount in the past year (USDT), update in 5 mins
totalWalletBalance	string	Wallet balance range
1: less than 100 USDT value
2: [100, 250) USDT value
3: [250, 500) USDT value
4: greater than 500 USDT value
depositUpdateTime	string	The update date time (UTC) of deposit data
volUpdateTime	string	The update date of volume data time (UTC)
KycLevel	integer	KYC level. 1, 2, 0
RUN >>
Request Example
HTTP
 
  
GET /v5/user/aff-customer-info?uid=1513500 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1685596324209
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: xxxxxx
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "uid": "1513500",
        "takerVol30Day": "10",
        "makerVol30Day": "20",
        "tradeVol30Day": "30",
        "depositAmount30Day": "90",
        "takerVol365Day": "100",
        "makerVol365Day": "500",
        "tradeVol365Day": "600",
        "depositAmount365Day": "1300",
        "totalWalletBalance": "4",
        "depositUpdateTime": "2023-06-01 05:12:04",
        "vipLevel": "99",
        "volUpdateTime": "2023-06-02 00:00:00",
        "KycLevel": 1
    },
    "retExtInfo": {},
    "time": 1685596324508
}

Get VIP Margin Data
This margin data is for Unified account in particular.

info
Does not need authentication.

HTTP Request
GET /v5/spot-margin-trade/data

Request Parameters
Parameter	Required	Type	Comments
vipLevel	false	string	Vip level
currency	false	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
vipCoinList	array	Object
> list	array	Object
>> borrowable	boolean	Whether it is allowed to be borrowed
>> collateralRatio	string	Due to the new Tiered Collateral value logic, this field will no longer be accurate starting on February 19, 2025. Please refer to Get Tiered Collateral Ratio
>> currency	string	Coin name
>> hourlyBorrowRate	string	Borrow interest rate per hour
>> liquidationOrder	string	Liquidation order
>> marginCollateral	boolean	Whether it can be used as a margin collateral currency
>> maxBorrowingAmount	string	Max borrow amount
> vipLevel	string	Vip level
RUN >>
Request Example
HTTP
 
  
GET /v5/spot-margin-trade/data?vipLevel=No VIP&currency=BTC HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "vipCoinList": [
            {
                "list": [
                    {
                        "borrowable": true,
                        "collateralRatio": "0.95",
                        "currency": "BTC",
                        "hourlyBorrowRate": "0.0000015021220000",
                        "liquidationOrder": "11",
                        "marginCollateral": true,
                        "maxBorrowingAmount": "3"
                    }
                ],
                "vipLevel": "No VIP"
            }
        ]
    }
}

Get Tiered Collateral Ratio
UTA loan tiered collateral ratio

info
Does not need authentication.

HTTP Request
GET /v5/spot-margin-trade/collateral

Request Parameters
Parameter	Required	Type	Comments
currency	false	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
list	array	Object
> currency	string	Coin name
> collateralRatioList	array	Object
>> maxQty	string	Upper limit(in coin) of the tiered range, "" means positive infinity
>> minQty	string	lower limit(in coin) of the tiered range
>> collateralRatio	string	Collateral ratio
Request Example
HTTP
 
  
GET /v5/spot-margin-trade/collateral?currency=BTC HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "currency": "BTC",
                "collateralRatioList": [
                    {
                        "minQty": "0",
                        "maxQty": "1000000",
                        "collateralRatio": "0.85"
                    },
                    {
                        "minQty": "1000000",
                        "maxQty": "",
                        "collateralRatio": "0"
                    }
                ]
            }
        ]
    },
    "retExtInfo": "{}",
    "time": 1739848984945
}

Get Historical Interest Rate
You can query up to six months borrowing interest rate of Margin trading.

info
Need authentication, the api key needs "Spot" permission
Only supports Unified account
It is public data, i.e., different users get the same historical interest rate for the same VIP/Pro
HTTP Request
GET /v5/spot-margin-trade/interest-rate-history

Request Parameters
Parameter	Required	Type	Comments
currency	true	string	Coin name, uppercase only
vipLevel	false	string	Vip level
Please note that "No VIP" should be passed like "No%20VIP" in the query string
If not passed, it returns your account's VIP level data
startTime	false	integer	The start timestamp (ms)
Either both time parameters are passed or neither is passed.
Returns 7 days data when both are not passed
Supports up to 30 days interval when both are passed
endTime	false	integer	The end timestamp (ms)
Response Parameters
Parameter	Type	Comments
list	array<object>	
> timestamp	long	timestamp
> currency	string	coin name
> hourlyBorrowRate	string	Hourly borrowing rate
> vipLevel	string	VIP/Pro level
Request Example
HTTP
 
GET /v5/spot-margin-trade/interest-rate-history?currency=USDC&vipLevel=No%20VIP&startTime=1721458800000&endTime=1721469600000 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1721891663064
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "list": [
            {
                "timestamp": 1721469600000,
                "currency": "USDC",
                "hourlyBorrowRate": "0.000014621596",
                "vipLevel": "No VIP"
            },
            {
                "timestamp": 1721466000000,
                "currency": "USDC",
                "hourlyBorrowRate": "0.000014621596",
                "vipLevel": "No VIP"
            },
            {
                "timestamp": 1721462400000,
                "currency": "USDC",
                "hourlyBorrowRate": "0.000014621596",
                "vipLevel": "No VIP"
            },
            {
                "timestamp": 1721458800000,
                "currency": "USDC",
                "hourlyBorrowRate": "0.000014621596",
                "vipLevel": "No VIP"
            }
        ]
    },
    "retExtInfo": "{}",
    "time": 1721899048991
}

Toggle Margin Trade
Turn on / off spot margin trade

Covers: Margin trade (Unified Account)

caution
Your account needs to activate spot margin first; i.e., you must have finished the quiz on web / app.

HTTP Request
POST /v5/spot-margin-trade/switch-mode

Request Parameters
Parameter	Required	Type	Comments
spotMarginMode	true	string	1: on, 0: off
Response Parameters
Parameter	Type	Comments
spotMarginMode	string	Spot margin status. 1: on, 0: off
RUN >>
Request Example
HTTP
 
  
POST /v5/spot-margin-trade/switch-mode HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672297794480
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "spotMarginMode": "0"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "spotMarginMode": "0"
    },
    "retExtInfo": {},
    "time": 1672297795542
}

Set Leverage
Set the user's maximum leverage in spot cross margin

Covers: Margin trade (Unified Account)

caution
Your account needs to activate spot margin first; i.e., you must have finished the quiz on web / app.

HTTP Request
POST /v5/spot-margin-trade/set-leverage

Request Parameters
Parameter	Required	Type	Comments
leverage	true	string	Leverage. [2, 10].
RUN >>
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/spot-margin-trade/set-leverage HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672299806626
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "leverage": "4"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {},
    "retExtInfo": {},
    "time": 1672710944282
}

Get Status And Leverage
Query the Spot margin status and leverage of Unified account

Covers: Margin trade (Unified Account)

HTTP Request
GET /v5/spot-margin-trade/state

Request Parameters
None

Response Parameters
Parameter	Type	Comments
spotLeverage	string	Spot margin leverage. Returns "" if the margin trade is turned off
spotMarginMode	string	Spot margin status. 1: on, 0: off
effectiveLeverage	string	actual leverage ratio. Precision retains 2 decimal places, truncate downwards
RUN >>
Request Example
HTTP
 
  
GET /v5/spot-margin-trade/state HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1692696840996
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "spotLeverage": "10",
        "spotMarginMode": "1",
        "effectiveLeverage": "1"
    },
    "retExtInfo": {},
    "time": 1692696841231
}

Get Borrowable Coins
info
Does not need authentication.

HTTP Request
GET /v5/crypto-loan-common/loanable-data

Request Parameters
Parameter	Required	Type	Comments
vipLevel	false	string	Vip level
VIP0, VIP1, VIP2, VIP3, VIP4, VIP5, VIP99(supreme VIP)
PRO1, PRO2, PRO3, PRO4, PRO5, PRO6
currency	false	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
list	array	Object
> currency	string	Coin name
> fixedBorrowable	boolean	Whether support fixed loan
> fixedBorrowingAccuracy	integer	Coin precision for fixed loan
> flexibleBorrowable	boolean	Whether support flexible loan
> flexibleBorrowingAccuracy	integer	Coin precision for flexible loan
> maxBorrowingAmount	string	Max borrow limit
> minFixedBorrowingAmount	string	Minimum amount for each fixed loan order
> minFlexibleBorrowingAmount	string	Minimum amount for each flexible loan order
> vipLevel	string	Vip level
> flexibleAnnualizedInterestRate	integer	The annualized interest rate for flexible borrowing. If the loan currency does not support flexible borrowing, it will always be """"
> annualizedInterestRate7D	string	The lowest annualized interest rate for fixed borrowing for 7 days that the market can currently provide. If there is no lending in the current market, then it is empty string
> annualizedInterestRate14D	string	The lowest annualized interest rate for fixed borrowing for 14 days that the market can currently provide. If there is no lending in the current market, then it is empty string
> annualizedInterestRate30D	string	The lowest annualized interest rate for fixed borrowing for 30 days that the market can currently provide. If there is no lending in the current market, then it is empty string
> annualizedInterestRate60D	string	The lowest annualized interest rate for fixed borrowing for 60 days that the market can currently provide. If there is no lending in the current market, then it is empty string
> annualizedInterestRate90D	string	The lowest annualized interest rate for fixed borrowing for 90 days that the market can currently provide. If there is no lending in the current market, then it is empty string
> annualizedInterestRate180D	string	The lowest annualized interest rate for fixed borrowing for 180 days that the market can currently provide. If there is no lending in the current market, then it is empty string
Request Example
HTTP
 
  
GET /v5/crypto-loan-common/loanable-data?currency=ETH&vipLevel=VIP5 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "currency": "ETH",
                "fixedBorrowable": true,
                "fixedBorrowingAccuracy": 6,
                "flexibleBorrowable": true,
                "flexibleBorrowingAccuracy": 4,
                "maxBorrowingAmount": "1100",
                "minFixedBorrowingAmount": "0.1",
                "minFlexibleBorrowingAmount": "0.001",
                "vipLevel": "VIP5",
                "annualizedInterestRate14D": "0.08",
                "annualizedInterestRate180D": "",
                "annualizedInterestRate30D": "",
                "annualizedInterestRate60D": "",
                "annualizedInterestRate7D": "",
                "annualizedInterestRate90D": "",
                "flexibleAnnualizedInterestRate": "0.001429799316"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1752573126653
}

Get Collateral Coins
info
Does not need authentication.

HTTP Request
GET /v5/crypto-loan-common/collateral-data

Request Parameters
Parameter	Required	Type	Comments
currency	false	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
collateralRatioConfigList	array	Object
> collateralRatioList	array	Object
>> collateralRatio	string	Collateral ratio
>> maxValue	string	Max qty
>> minValue	string	Min qty
> currencies	string	Currenies with the same collateral ratio, e.g., BTC,ETH,XRP
currencyLiquidationList	array	Object
> currency	string	Coin name
> liquidationOrder	integer	Liquidation order
Request Example
HTTP
 
  
GET /v5/crypto-loan-common/collateral-data?currency=BTC HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "collateralRatioConfigList": [
            {
                "collateralRatioList": [
                    {
                        "collateralRatio": "0.8",
                        "maxValue": "10000",
                        "minValue": "0"
                    },
                    {
                        "collateralRatio": "0.7",
                        "maxValue": "20000",
                        "minValue": "10000"
                    },
                    {
                        "collateralRatio": "0.5",
                        "maxValue": "30000",
                        "minValue": "20000"
                    },
                    {
                        "collateralRatio": "0.4",
                        "maxValue": "99999999999",
                        "minValue": "30000"
                    }
                ],
                "currencies": "ATOM,AAVE,BTC,BOB"
            }
        ],
        "currencyLiquidationList": [
            {
                "currency": "BTC",
                "liquidationOrder": 1
            }
        ]
    },
    "retExtInfo": {},
    "time": 1752627381571
}

Get Max. Allowed Collateral Reduction Amount
Retrieve the maximum redeemable amount of your collateral asset based on LTV.

Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-common/max-collateral-amount

Request Parameters
Parameter	Required	Type	Comments
currency	true	string	Collateral coin
Response Parameters
Parameter	Type	Comments
maxCollateralAmount	string	Maximum reduction amount
Request Example
HTTP
 
  
GET /v5/crypto-loan-common/max-collateral-amount?currency=BTC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752627687351
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "maxCollateralAmount": "0.08585184"
    },
    "retExtInfo": {},
    "time": 1752627687596
}

Adjust Collateral Amount
You can increase or reduce your collateral amount. When you reduce, please obey the Get Max. Allowed Collateral Reduction Amount

Permission: "Spot trade"
UID rate limit: 1 req / second

info
The adjusted collateral amount will be returned to or deducted from the Funding wallet.
HTTP Request
POST /v5/crypto-loan-common/adjust-ltv

Request Parameters
Parameter	Required	Type	Comments
currency	true	string	Collateral coin
amount	true	string	Adjustment amount
direction	true	string	0: add collateral; 1: reduce collateral
Response Parameters
Parameter	Type	Comments
adjustId	long	Collateral adjustment transaction ID
Request Example
HTTP
 
  
POST /v5/crypto-loan-common/adjust-ltv HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752627997649
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 69

{
    "currency": "BTC",
    "amount": "0.08",
    "direction": "1"
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "adjustId": 27511
    },
    "retExtInfo": {},
    "time": 1752627997915
}

Get Collateral Adjustment History
Query for your LTV adjustment history.

Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-common/adjustment-history

Request Parameters
Parameter	Required	Type	Comments
adjustId	false	string	Collateral adjustment transaction ID
collateralCurrency	false	string	Collateral coin name
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> collateralCurrency	string	Collateral coin
> amount	string	amount
> adjustId	long	Collateral adjustment transaction ID
> adjustTime	long	Adjust timestamp
> preLTV	string	LTV before the adjustment
> afterLTV	string	LTV after the adjustment
> direction	integer	The direction of adjustment, 0: add collateral; 1: reduce collateral
> status	integer	The status of adjustment, 1: success; 2: processing; 3: fail
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-common/adjustment-history?limit=2&collateralCurrency=BTC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752628288472
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "adjustId": 27511,
                "adjustTime": 1752627997907,
                "afterLTV": "0.813743",
                "amount": "0.08",
                "collateralCurrency": "BTC",
                "direction": 1,
                "preLTV": "0.524602",
                "status": 1
            },
            {
                "adjustId": 27491,
                "adjustTime": 1752218558913,
                "afterLTV": "0.41983",
                "amount": "0.03",
                "collateralCurrency": "BTC",
                "direction": 1,
                "preLTV": "0.372314",
                "status": 1
            }
        ],
        "nextPageCursor": "27491"
    },
    "retExtInfo": {},
    "time": 1752628288732
}

Get Cryptp Loan Position
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-common/position

Request Parameters
None

Response Parameters
Parameter	Type	Comments
borrowList	array	Object
> fixedTotalDebt	string	Total debt of fixed loan (coin)
> fixedTotalDebtUSD	string	Total debt of fixed loan (USD)
> flexibleHourlyInterestRate	string	Flebible loan hourly interest rate
> flexibleTotalDebt	string	Total debt of flexible loan (coin)
> flexibleTotalDebtUSD	string	Total debt of flexible loan (USD)
> loanCurrency	string	Loan coin
collateralList	array	Object
> amount	string	Collateral amount in coin
> amountUSD	string	Collateral amount in USD (after tierd collateral ratio calculation)
> currency	string	Collateral coin
ltv	string	LTV
supplyList	array	Object
> amount	string	Supply amount in coin
> amountUSD	string	Supply amount in USD
> currency	string	Supply coin
totalCollateral	string	Total collateral amount (USD)
totalDebt	string	Total debt (fixed + flexible, in USD)
totalSupply	string	Total supply amount (USD)
Request Example
HTTP
 
  
GET /v5/crypto-loan-common/position HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752628288472
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "borrowList": [
            {
                "fixedTotalDebt": "0",
                "fixedTotalDebtUSD": "0",
                "flexibleHourlyInterestRate": "0.0000001361462",
                "flexibleTotalDebt": "0.08800022",
                "flexibleTotalDebtUSD": "9355.37",
                "loanCurrency": "BTC"
            },
            {
                "fixedTotalDebt": "0.1",
                "fixedTotalDebtUSD": "282.8",
                "flexibleHourlyInterestRate": "0.00000188498892",
                "flexibleTotalDebt": "0",
                "flexibleTotalDebtUSD": "0",
                "loanCurrency": "ETH"
            }
        ],
        "collateralList": [
            {
                "amount": "0.12",
                "amountUSD": "9930.11",
                "currency": "BTC"
            },
            {
                "amount": "2",
                "amountUSD": "4524.81",
                "currency": "ETH"
            },
            {
                "amount": "4002.12",
                "amountUSD": "3201.69",
                "currency": "USDT"
            },
            {
                "amount": "1000",
                "amountUSD": "724.8",
                "currency": "USDC"
            }
        ],
        "ltv": "0.524344",
        "supplyList": [
            {
                "amount": "800.13041095890410959",
                "amountUSD": "800.13",
                "currency": "USDT"
            }
        ],
        "totalCollateral": "18381.41",
        "totalDebt": "9638.17",
        "totalSupply": "800.13"
    },
    "retExtInfo": {},
    "time": 1752627962000
}

Borrow
Permission: "Spot trade"
UID rate limit: 1 req / second

info
The loan funds are released to the Funding wallet.
The collateral funds are deducted from the Funding wallet, so make sure you have enough collateral amount in the Funding wallet.
HTTP Request
POST /v5/crypto-loan-flexible/borrow

Request Parameters
Parameter	Required	Type	Comments
loanCurrency	true	string	Loan coin name
loanAmount	true	string	Amount to borrow
collateralList	false	array<object>	Collateral coin list, supports putting up to 100 currency in the array
> collateralCurrency	false	string	Currency used to mortgage
> collateralAmount	false	string	Amount to mortgage
Response Parameters
Parameter	Type	Comments
orderId	string	Loan order ID
Request Example
HTTP
 
  
POST /v5/crypto-loan-flexible/borrow HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752569210041
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 244

{
    "loanCurrency": "BTC",
    "loanAmount": "0.1",
    "collateralList": [
        {
            "currency": "USDT",
            "amount": "1000"
        },
        {
            "currency": "ETH",
            "amount": "1"
        }
    ]
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "orderId": "1363"
    },
    "retExtInfo": {},
    "time": 1752569209682
}

Repay
Fully or partially repay a loan. If interest is due, that is paid off first, with the loaned amount being paid off only after due interest.

Permission: "Spot trade"
UID rate limit: 1 req / second

info
The repaid amount will be deducted from the Funding wallet.
The collateral amount will not be auto returned when you don't fully repay the debt, but you can also adjust collateral amount
HTTP Request
POST /v5/crypto-loan-flexible/repay

Request Parameters
Parameter	Required	Type	Comments
loanCurrency	true	string	Loan currency
amount	true	string	Repay amount
Response Parameters
Parameter	Type	Comments
repayId	string	Repayment transaction ID
Request Example
HTTP
 
  
POST /v5/crypto-loan-flexible/repay HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752569628364
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 52

{
    "loanCurrency": "BTC",
    "amount": "0.005"
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "repayId": "1771"
    },
    "retExtInfo": {},
    "time": 1752569614549
}

Get Flexible loans
Query for your on  ing loans

Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-flexible/on  ing-coin

Request Parameters
Parameter	Required	Type	Comments
loanCurrency	false	string	Loan coin name
Response Parameters
Parameter	Type	Comments
list	array	Object
> hourlyInterestRate	string	Latest hourly flexible interest rate
> loanCurrency	string	Loan coin
> totalDebt	string	Unpaid principal and interest
Request Example
HTTP
 
  
GET /v5/crypto-loan-flexible/on  ing-coin?loanCurrency=BTC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752570124973
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "hourlyInterestRate": "0.00000013597784",
                "loanCurrency": "BTC",
                "totalDebt": "0.10100003"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1752570124242
}

Get Borrow Orders History
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-flexible/borrow-history

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID
loanCurrency	false	string	Loan coin name
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> borrowTime	long	The timestamp to borrow
> initialLoanAmount	string	Loan amount
> loanCurrency	string	Loan coin
> orderId	string	Loan order ID
> status	integer	Loan order status 1: success; 2: processing; 3: fail
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-flexible/borrow-history?limit=2 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752570519918
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "borrowTime": 1752569950643,
                "initialLoanAmount": "0.006",
                "loanCurrency": "BTC",
                "orderId": "1364",
                "status": 1
            },
            {
                "borrowTime": 1752569209643,
                "initialLoanAmount": "0.1",
                "loanCurrency": "BTC",
                "orderId": "1363",
                "status": 1
            }
        ],
        "nextPageCursor": "1363"
    },
    "retExtInfo": {},
    "time": 1752570519414
}

Get Repayment Orders History
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-flexible/repayment-history

Request Parameters
Parameter	Required	Type	Comments
repayId	false	string	Repayment tranaction ID
loanCurrency	false	string	Loan coin name
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> loanCurrency	string	Loan coin
> repayAmount	string	Repayment amount
> repayId	string	Repayment transaction ID
> repayStatus	integer	Repayment status, 1: success; 2: processing; 3: fail
> repayTime	long	Repay timestamp
> repayType	integer	Repayment type, 1: repay by user; 2: repay by liquidation; 5: repay by delisting; 6: repay by delay liquidation
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-flexible/repayment-history?loanCurrency=BTC HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752570746227
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "loanCurrency": "BTC",
                "repayAmount": "0.007",
                "repayId": "1773",
                "repayStatus": 1,
                "repayTime": 1752570731274,
                "repayType": 1
            },
            {
                "loanCurrency": "BTC",
                "repayAmount": "0.006",
                "repayId": "1772",
                "repayStatus": 1,
                "repayTime": 1752570726038,
                "repayType": 1
            },
            {
                "loanCurrency": "BTC",
                "repayAmount": "0.005",
                "repayId": "1771",
                "repayStatus": 1,
                "repayTime": 1752569614528,
                "repayType": 1
            }
        ],
        "nextPageCursor": "1769"
    },
    "retExtInfo": {},
    "time": 1752570745493
}

Get Supplying Market
info
Does not need authentication.

If you want to supply, you can use this endpoint to check whether there are any suitable counterparty borrow orders available.

HTTP Request
GET /v5/crypto-loan-fixed/supply-order-quote

Request Parameters
Parameter	Required	Type	Comments
orderCurrency	true	string	Coin name
term	false	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
orderBy	true	string	Order by, apy: annual rate; term; quantity
sort	false	integer	0: ascend, default; 1: descend
limit	false	integer	Limit for data size per page. [1, 100]. Default: 10
Response Parameters
Parameter	Type	Comments
list	array	Object
> orderCurrency	string	Coin name
> term	integer	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
> annualRate	string	Annual rate
> qty	string	Quantity
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/supply-order-quote?orderCurrency=USDT&orderBy=apy HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "annualRate": "0.02",
                "orderCurrency": "USDT",
                "qty": "1000.1234",
                "term": 60
            },
            {
                "annualRate": "0.022",
                "orderCurrency": "USDT",
                "qty": "212.1234",
                "term": 7
            }
        ]
    },
    "retExtInfo": {},
    "time": 1752652136224
}

Get Borrowing Market
info
Does not need authentication.

If you want to borrow, you can use this endpoint to check whether there are any suitable counterparty supply orders available.

HTTP Request
GET /v5/crypto-loan-fixed/borrow-order-quote

Request Parameters
Parameter	Required	Type	Comments
orderCurrency	true	string	Coin name
term	false	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
orderBy	true	string	Order by, apy: annual rate; term; quantity
sort	false	integer	0: ascend, default; 1: descend
limit	false	integer	Limit for data size per page. [1, 100]. Default: 10
Response Parameters
Parameter	Type	Comments
list	array	Object
> orderCurrency	string	Coin name
> term	integer	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
> annualRate	string	Annual rate
> qty	string	Quantity
Request Example
HTTP
 
  
GET /v5/crypto-loan-common/loanable-data?currency=ETH&vipLevel=VIP5 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "annualRate": "0.04",
                "orderCurrency": "USDT",
                "qty": "988.78",
                "term": 14
            }
        ]
    },
    "retExtInfo": {},
    "time": 1752719158890
}

Create Borrow Order
Permission: "Spot trade"
UID rate limit: 1 req / second

info
The loan funds are released to the Funding wallet.
The collateral funds are deducted from the Funding wallet, so make sure you have enough collateral amount in the Funding wallet.
HTTP Request
POST /v5/crypto-loan-fixed/borrow

Request Parameters
Parameter	Required	Type	Comments
orderCurrency	true	string	Currency to borrow
orderAmount	true	string	Amount to borrow
annualRate	true	string	Customizable annual interest rate, e.g., 0.02 means 2%
term	true	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
autoRepay	false	string	Enable Auto-Repay to have assets in your Funding Account automatically repay your loan upon Borrowing order expiration, preventing overdue penalties. Ensure your Funding Account maintains sufficient amount for repayment to avoid automatic repayment failures.
"true": enable, default; "false": disable
collateralList	false	array<object>	Collateral coin list, supports putting up to 100 currency in the array
> currency	false	string	Currency used to mortgage
> amount	false	string	Amount to mortgage
Response Parameters
Parameter	Type	Comments
orderId	string	Loan order ID
Request Example
HTTP
 
  
POST /v5/crypto-loan-fixed/borrow HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752633649752
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 208

{
    "orderCurrency": "ETH",
    "orderAmount": "1.5",
    "annualRate": "0.022",
    "term": "30",
    "autoRepay": "true",
    "collateralList": {
        "currency": "BTC",
        "amount": "0.1"
    }
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "orderId": "13007"
    },
    "retExtInfo": {},
    "time": 1752633650147
}

Create Supply Order
Permission: "Spot trade"
UID rate limit: 1 req / second

HTTP Request
POST /v5/crypto-loan-fixed/supply

Request Parameters
Parameter	Required	Type	Comments
orderCurrency	true	string	Currency to supply
orderAmount	true	string	Amount to supply
annualRate	true	string	Customizable annual interest rate, e.g., 0.02 means 2%
term	true	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
Response Parameters
Parameter	Type	Comments
orderId	string	Supply order ID
Request Example
HTTP
 
  
POST /v5/crypto-loan-fixed/supply HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752652261840
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 104

{
    "orderCurrency": "USDT",
    "orderAmount": "2002.21",
    "annualRate": "0.35",
    "term": "7"
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "orderId": "13007"
    },
    "retExtInfo": {},
    "time": 1752633650147
}

Cancel Borrow Order
Permission: "Spot trade"
UID rate limit: 1 req / second

HTTP Request
POST /v5/crypto-loan-fixed/borrow-order-cancel

Request Parameters
Parameter	Required	Type	Comments
orderId	true	string	Order ID of fixed borrow order
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/crypto-loan-fixed/borrow-order-cancel HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752652457987
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 26

{
    "orderId": "13009"
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {},
    "retExtInfo": {},
    "time": 1752652458684
}

Cancel Supply Order
Permission: "Spot trade"
UID rate limit: 1 req / second

HTTP Request
POST /v5/crypto-loan-fixed/supply-order-cancel

Request Parameters
Parameter	Required	Type	Comments
orderId	true	string	Order ID of fixed supply order
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/crypto-loan-fixed/supply-order-cancel HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752652612736
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 26

{
    "orderId": "13577"
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {},
    "retExtInfo": {},
    "time": 1752652613638
}

Get Borrow Contract Info
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-fixed/borrow-contract-info

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID
loanId	false	string	Loan ID
orderCurrency	false	string	Loan coin name
term	false	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> annualRate	string	Annual rate for the borrowing
> autoRepay	string	"true": enable auto repay, default; "false": disable auto repay
> borrowCurrency	string	Loan coin
> borrowTime	string	Loan order timestamp
> interestPaid	string	Paid interest
> loanId	string	Loan contract ID
> orderId	string	Loan order ID
> repaymentTime	string	Time to repay
> residualPenaltyInterest	string	Unpaid interest
> residualPrincipal	string	Unpaid principal
> status	integer	Loan order status 1: unrepaid; 2: fully repaid; 3: overdue
> term	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/borrow-contract-info?orderCurrency=ETH HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752652691909
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "annualRate": "0.022",
                "autoRepay": "true",
                "borrowCurrency": "ETH",
                "borrowTime": "1752633756068",
                "interestPaid": "0.002531506849315069",
                "loanId": "571",
                "orderId": "13007",
                "repaymentTime": "1755225756068",
                "residualPenaltyInterest": "0",
                "residualPrincipal": "1.4",
                "status": 1,
                "term": "30"
            },
            {
                "annualRate": "0.022",
                "autoRepay": "true",
                "borrowCurrency": "ETH",
                "borrowTime": "1752633696068",
                "interestPaid": "0.00018082191780822",
                "loanId": "570",
                "orderId": "13007",
                "repaymentTime": "1755225696068",
                "residualPenaltyInterest": "0",
                "residualPrincipal": "0.1",
                "status": 1,
                "term": "30"
            }
        ],
        "nextPageCursor": "568"
    },
    "retExtInfo": {},
    "time": 1752652692603
}

Get Supply Contract Info
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-fixed/supply-contract-info

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Supply order ID
supplyId	false	string	Supply contract ID
supplyCurrency	false	string	Supply coin name
term	false	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> annualRate	string	Annual rate for the supply
> supplyCurrency	string	Supply coin
> supplyTime	string	Supply timestamp
> supplyAmount	string	Supply amount
> interestPaid	string	Paid interest
> supplyId	string	Supply contract ID
> orderId	string	Supply order ID
> redemptionTime	string	Planned time to redeem
> penaltyInterest	string	Overdue interest
> actualRedemptionTime	string	Actual time to redeem
> status	integer	Supply contract status 1: Supplying; 2: Redeemed
> term	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/supply-contract-info?supplyCurrency=USDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752654376532
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "actualRedemptionTime": "1753087596082",
                "annualRate": "0.01",
                "interest": "0.13041095890410959",
                "orderId": "13564",
                "penaltyInterest": "0",
                "redemptionTime": "1753087596082",
                "status": 1,
                "supplyAmount": "800",
                "supplyCurrency": "USDT",
                "supplyId": "567",
                "supplyTime": "1752482796082",
                "term": "7"
            }
        ],
        "nextPageCursor": "567"
    },
    "retExtInfo": {},
    "time": 1752654377461
}

Get Borrow Order Info
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-fixed/borrow-order-info

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID
orderCurrency	false	string	Loan coin name
state	false	string	Borrow order status, 1: matching; 2: partially filled and cancelled; 3: Fully filled; 4: Cancelled
term	false	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> annualRate	string	Annual rate for the borrowing
> orderId	long	Loan order ID
> orderTime	string	Order created time
> filledQty	string	Filled qty
> orderQty	string	Order qty
> orderCurrency	string	Coin name
> state	integer	Borrow order status, 1: matching; 2: partially filled and cancelled; 3: Fully filled; 4: Cancelled; 5: fail
> term	integer	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/borrow-order-info?orderId=13010 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752655239825
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "annualRate": "0.01",
                "filledQty": "0",
                "orderCurrency": "MANA",
                "orderId": 13010,
                "orderQty": "2000",
                "orderTime": "1752654035179",
                "state": 1,
                "term": 30
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1752655241090
}

Get Supply Order Info
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-fixed/supply-order-info

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Supply order ID
orderCurrency	false	string	Supply coin name
state	false	string	Supply order status, 1: matching; 2: partially filled and cancelled; 3: Fully filled; 4: Cancelled
term	false	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> annualRate	string	Annual rate for the supply
> orderId	long	Supply order ID
> orderTime	string	Order created time
> filledQty	string	Filled qty
> orderQty	string	Order qty
> orderCurrency	string	Coin name
> state	integer	Supply order status, 1: matching; 2: partially filled and cancelled; 3: Fully filled; 4: Cancelled; 5: fail
> term	integer	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/supply-order-info?orderId=13564 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752655992606
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "annualRate": "0.01",
                "filledQty": "800",
                "orderCurrency": "USDT",
                "orderId": 13564,
                "orderQty": "1020",
                "orderTime": "1752482751043",
                "state": 2,
                "term": 7
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1752655993869
}

Repay
Permission: "Spot trade"
UID rate limit: 1 req / second

HTTP Request
POST /v5/crypto-loan-fixed/fully-repay

Request Parameters
Parameter	Required	Type	Comments
loanId	false	string	Loan contract ID. Either loanId or loanCurrency needs to be passed
loanCurrency	false	string	Loan coin. Either loanId or loanCurrency needs to be passed
Response Parameters
Parameter	Type	Comments
repayId	string	Repayment transaction ID
Request Example
HTTP
 
  
POST /v5/crypto-loan-fixed/fully-repay HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752656296791
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 50

{
    "loanId": "570",
    "loanCurrency": "ETH"
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "repayId": "1771"
    },
    "retExtInfo": {},
    "time": 1752569614549
}

Get Repayment History
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-fixed/repayment-history

Request Parameters
Parameter	Required	Type	Comments
repayId	false	string	Repayment order ID
loanCurrency	false	string	Loan coin name
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> details	array	Object
>> loanCurrency	string	Loan coin name
>> repayAmount	long	Repay amount
>> loanId	string	Loan ID. One repayment may involve multiple loan contracts.
> loanCurrency	string	Loan coin name
> repayAmount	long	Repay amount
> repayId	string	Repay order ID
> repayStatus	integer	Status, 1: success, 2: processing, 3: fail
> repayTime	long	Repay time
> repayType	integer	Repay type, 1: repay by user; 2: repay by liquidation; 3: auto repay; 4: overdue repay; 5: repay by delisting; 6: repay by delay liquidation
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/repayment-history?repayId=1780 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: XXXXXXX
X-BAPI-TIMESTAMP: 1752714738425
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "details": [
                    {
                        "loanCurrency": "ETH",
                        "loanId": "568",
                        "repayAmount": "0.1"
                    },
                    {
                        "loanCurrency": "ETH",
                        "loanId": "571",
                        "repayAmount": "1.4"
                    }
                ],
                "loanCurrency": "ETH",
                "repayAmount": "1.5",
                "repayId": "1782",
                "repayStatus": 1,
                "repayTime": 1752717174353,
                "repayType": 1
            }
        ],
        "nextPageCursor": "1674"
    },
    "retExtInfo": {},
    "time": 1752717183557
}

Get Collateral Coins
info
Does not need authentication.

HTTP Request
GET /v5/crypto-loan/collateral-data

Request Parameters
Parameter	Required	Type	Comments
vipLevel	false	string	Vip level
VIP0, VIP1, VIP2, VIP3, VIP4, VIP5, VIP99(supreme VIP)
PRO1, PRO2, PRO3, PRO4, PRO5, PRO6
currency	false	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
vipCoinList	array	Object
> list	array	Object
>> collateralAccuracy	integer	Valid collateral coin precision
>> initialLTV	string	The Initial LTV ratio determines the initial amount of coins that can be borrowed. The initial LTV ratio may vary for different collateral
>> marginCallLTV	string	If the LTV ratio (Loan Amount/Collateral Amount) reaches the threshold, you will be required to add more collateral to your loan
>> liquidationLTV	string	If the LTV ratio (Loan Amount/Collateral Amount) reaches the threshold, Bybit will liquidate your collateral assets to repay your loan and interest in full
>> maxLimit	string	Collateral limit
> vipLevel	string	Vip level
Request Example
HTTP
 
  
GET /v5/crypto-loan/collateral-data?currency=ETH&vipLevel=PRO1 HTTP/1.1
Host: api.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "vipCoinList": [
            {
                "list": [
                    {
                        "collateralAccuracy": 8,
                        "currency": "ETH",
                        "initialLTV": "0.8",
                        "liquidationLTV": "0.95",
                        "marginCallLTV": "0.87",
                        "maxLimit": "32000"
                    }
                ],
                "vipLevel": "PRO1"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1728618590498
}

Get Borrowable Coins
info
Does not need authentication.

danger
Borrowed coins can be returned at any time before the due date. You'll be charged 3 times the hourly interest during the overdue period. Your collateral will be liquidated to repay a loan and the interest if you fail to make the repayment 48 hours after the due time.

HTTP Request
GET /v5/crypto-loan/loanable-data

Request Parameters
Parameter	Required	Type	Comments
vipLevel	false	string	Vip level
VIP0, VIP1, VIP2, VIP3, VIP4, VIP5, VIP99(supreme VIP)
PRO1, PRO2, PRO3, PRO4, PRO5, PRO6
currency	false	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
vipCoinList	array	Object
> list	array	Object
>> borrowingAccuracy	integer	The number of decimal places (precision) of this coin
>> currency	string	Coin name
>> flexibleHourlyInterestRate	string	Flexible hourly floating interest rate
Flexible Crypto Loans offer an hourly floating interest rate, calculated based on the actual borrowing time per hour, with the option for early repayment
Is "" if the coin does not support flexible loan
>> hourlyInterestRate7D	string	Hourly interest rate for 7 days loan. Is "" if the coin does not support 7 days loan
>> hourlyInterestRate14D	string	Hourly interest rate for 14 days loan. Is "" if the coin does not support 14 days loan
>> hourlyInterestRate30D	string	Hourly interest rate for 30 days loan. Is "" if the coin does not support 30 days loan
>> hourlyInterestRate90D	string	Hourly interest rate for 90 days loan. Is "" if the coin does not support 90 days loan
>> hourlyInterestRate180D	string	Hourly interest rate for 180 days loan. Is "" if the coin does not support 180 days loan
>> maxBorrowingAmount	string	Max. amount to borrow
>> minBorrowingAmount	string	Min. amount to borrow
> vipLevel	string	Vip level
Request Example
HTTP
 
  
GET /v5/crypto-loan/loanable-data?currency=USDT&vipLevel=VIP0 HTTP/1.1
Host: api.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "vipCoinList": [
            {
                "list": [
                    {
                        "borrowingAccuracy": 4,
                        "currency": "USDT",
                        "flexibleHourlyInterestRate": "0.0000090346",
                        "hourlyInterestRate14D": "0.0000207796",
                        "hourlyInterestRate180D": "",
                        "hourlyInterestRate30D": "0.00002349",
                        "hourlyInterestRate7D": "0.0000180692",
                        "hourlyInterestRate90D": "",
                        "maxBorrowingAmount": "8000000",
                        "minBorrowingAmount": "20"
                    }
                ],
                "vipLevel": "VIP0"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1728619315868
}

Get Account Borrowable/Collateralizable Limit
Query for the minimum and maximum amounts your account can borrow and how much collateral you can put up.

Permission: "Spot trade"

HTTP Request
GET /v5/crypto-loan/borrowable-collateralisable-number

Request Parameters
Parameter	Required	Type	Comments
loanCurrency	true	string	Loan coin name
collateralCurrency	true	string	Collateral coin name
Response Parameters
Parameter	Type	Comments
collateralCurrency	string	Collateral coin name
loanCurrency	string	Loan coin name
maxCollateralAmount	string	Max. limit to mortgage
maxLoanAmount	string	Max. limit to borrow
minCollateralAmount	string	Min. limit to mortgage
minLoanAmount	string	Min. limit to borrow
Request Example
HTTP
 
  
GET /v5/crypto-loan/borrowable-collateralisable-number?loanCurrency=USDT&collateralCurrency=BTC HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728627083198
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "collateralCurrency": "BTC",
        "loanCurrency": "USDT",
        "maxCollateralAmount": "164.957732055526752104",
        "maxLoanAmount": "8000000",
        "minCollateralAmount": "0.000412394330138818",
        "minLoanAmount": "20"
    },
    "retExtInfo": {},
    "time": 1728627084863
}

Borrow
Permission: "Spot trade"

info
The loan funds are released to the Funding wallet.
The collateral funds are deducted from the Funding wallet, so make sure you have enough collateral amount in the Funding wallet.
HTTP Request
POST /v5/crypto-loan/borrow

Request Parameters
Parameter	Required	Type	Comments
loanCurrency	true	string	Loan coin name
loanAmount	false	string	Amount to borrow
Required when collateral amount is not filled
loanTerm	false	string	Loan term
flexible term: null or not passed
fixed term: 7, 14, 30, 90, 180 days
collateralCurrency	true	string	Currency used to mortgage
collateralAmount	false	string	Amount to mortgage
Required when loan amount is not filled
Response Parameters
Parameter	Type	Comments
orderId	string	Loan order ID
Request Example
HTTP
 
  
POST /v5/crypto-loan/borrow HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728629356551
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 140

{
    "loanCurrency": "USDT",
    "loanAmount": "550",
    "collateralCurrency": "BTC",
    "loanTerm": null,
    "collateralAmount": null
}

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "orderId": "1794267532472646144"
    },
    "retExtInfo": {},
    "time": 1728629357820
}


Repay
Fully or partially repay a loan. If interest is due, that is paid off first, with the loaned amount being paid off only after due interest.

Permission: "Spot trade"

info
The repaid amount will be deducted from the Funding wallet.
The collateral amount will not be auto returned when you don't fully repay the debt, but you can also adjust collateral amount
HTTP Request
POST /v5/crypto-loan/repay

Request Parameters
Parameter	Required	Type	Comments
orderId	true	string	Loan order ID
amount	true	string	Repay amount
Response Parameters
Parameter	Type	Comments
repayId	string	Repayment transaction ID
Request Example
HTTP
 
  
POST /v5/crypto-loan/repay HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728629785224
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 61

{
    "orderId": "1794267532472646144",
    "amount": "100"
}

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "repayId": "1794271131730737664"
    },
    "retExtInfo": {},
    "time": 1728629786884
}

Get Unpaid Loans
Query for your on  ing loans.

Permission: "Spot trade"

HTTP Request
GET /v5/crypto-loan/on  ing-orders

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID
loanCurrency	false	string	Loan coin name
collateralCurrency	false	string	Collateral coin name
loanTermType	false	string	
1: fixed term, when query this type, loanTerm must be filled
2: flexible term
By default, query all types
loanTerm	false	string	7, 14, 30, 90, 180 days, working when loanTermType=1
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> collateralAmount	string	Collateral amount
> collateralCurrency	string	Collateral coin
> currentLTV	string	Current LTV
> expirationTime	string	Loan maturity time, keeps "" for flexible loan
> hourlyInterestRate	string	Hourly interest rate
Flexible loan, it is real-time interest rate
Fixed term loan: it is fixed term interest rate
> loanCurrency	string	Loan coin
> loanTerm	string	Loan term, 7, 14, 30, 90, 180 days, keep "" for flexible loan
> orderId	string	Loan order ID
> residualInterest	string	Unpaid interest
> residualPenaltyInterest	string	Unpaid penalty interest
> totalDebt	string	Unpaid principal
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan/on  ing-orders?orderId=1793683005081680384 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728630979731
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "list": [
            {
                "collateralAmount": "0.0964687",
                "collateralCurrency": "BTC",
                "currentLTV": "0.4161",
                "expirationTime": "1731149999000",
                "hourlyInterestRate": "0.0000010633",
                "loanCurrency": "USDT",
                "loanTerm": "30",
                "orderId": "1793683005081680384",
                "residualInterest": "0.04016",
                "residualPenaltyInterest": "0",
                "totalDebt": "1888.005198"
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1728630980861
}

Get Loan Repayment History
Query for loan repayment transactions. A loan may be repaid in multiple repayments.

Permission: "Spot trade"

info
Supports querying for the last 6 months worth of completed loan orders.
Only successful repayments can be queried for.
HTTP Request
GET /v5/crypto-loan/repayment-history

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID
repayId	false	string	Repayment tranaction ID
loanCurrency	false	string	Loan coin name
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> collateralCurrency	string	Collateral coin
> collateralReturn	string	Amount of collateral returned as a result of this repayment. "0" if this isn't the final loan repayment
> loanCurrency	string	Loan coin
> loanTerm	string	Loan term, 7, 14, 30, 90, 180 days, keep "" for flexible loan
> orderId	string	Loan order ID
> repayAmount	string	Repayment amount
> repayId	string	Repayment transaction ID
> repayStatus	integer	Repayment status, 1: success; 2: processing
> repayTime	string	Repay timestamp
> repayType	string	Repayment type, 1: repay by user; 2: repay by liquidation
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan/repayment-history?repayId=1794271131730737664 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728633716794
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "list": [
            {
                "collateralCurrency": "BTC",
                "collateralReturn": "0",
                "loanCurrency": "USDT",
                "loanTerm": "",
                "orderId": "1794267532472646144",
                "repayAmount": "100",
                "repayId": "1794271131730737664",
                "repayStatus": 1,
                "repayTime": "1728629786875",
                "repayType": "1"
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1728633717935
}

Get Completed Loan History
Query for the last 6 months worth of your completed (fully paid off) loans.

Permission: "Spot trade"

HTTP Request
GET /v5/crypto-loan/borrow-history

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID
loanCurrency	false	string	Loan coin name
collateralCurrency	false	string	Collateral coin name
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> borrowTime	string	The timestamp to borrow
> collateralCurrency	string	Collateral coin
> expirationTime	string	Loan maturity time, keeps "" for flexible loan
> hourlyInterestRate	string	Hourly interest rate
Flexible loan, it is real-time interest rate
Fixed term loan: it is fixed term interest rate
> initialCollateralAmount	string	Initial amount to mortgage
> initialLoanAmount	string	Initial loan amount
> loanCurrency	string	Loan coin
> loanTerm	string	Loan term, 7, 14, 30, 90, 180 days, keep "" for flexible loan
> orderId	string	Loan order ID
> repaidInterest	string	Total interest repaid
> repaidPenaltyInterest	string	Total penalty interest repaid
> status	integer	Loan order status 1: fully repaid manually; 2: fully repaid by liquidation
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan/borrow-history?orderId=1793683005081680384 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728630979731
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "list": [
            {
                "borrowTime": "1728546174028",
                "collateralCurrency": "BTC",
                "expirationTime": "1729148399000",
                "hourlyInterestRate": "0.0000010241",
                "initialCollateralAmount": "0.0494727",
                "initialLoanAmount": "1",
                "loanCurrency": "ETH",
                "loanTerm": "7",
                "orderId": "1793569729874260992",
                "repaidInterest": "0.00000515",
                "repaidPenaltyInterest": "0",
                "status": 1
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1728632014857
}

Get Max. Allowed Collateral Reduction Amount
Query for the maximum amount by which collateral may be reduced by.

Permission: "Spot trade"

HTTP Request
GET /v5/crypto-loan/max-collateral-amount

Request Parameters
Parameter	Required	Type	Comments
orderId	true	string	Loan coin ID
Response Parameters
Parameter	Type	Comments
maxCollateralAmount	string	Max. reduction collateral amount
Request Example
HTTP
 
  
GET /v5/crypto-loan/max-collateral-amount?orderId=1794267532472646144 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728634289933
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "maxCollateralAmount": "0.00210611"
    },
    "retExtInfo": {},
    "time": 1728634291554
}

Adjust Collateral Amount
You can increase or reduce your collateral amount. When you reduce, please obey the max. allowed reduction amount.

Permission: "Spot trade"

info
The adjusted collateral amount will be returned to or deducted from the Funding wallet.
HTTP Request
POST /v5/crypto-loan/adjust-ltv

Request Parameters
Parameter	Required	Type	Comments
orderId	true	string	Loan order ID
amount	true	string	Adjustment amount
direction	true	string	0: add collateral; 1: reduce collateral
Response Parameters
Parameter	Type	Comments
adjustId	string	Collateral adjustment transaction ID
Request Example
HTTP
 
  
POST /v5/crypto-loan/adjust-ltv HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728635421137
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 85

{
    "orderId": "1794267532472646144",
    "amount": "0.001",
    "direction": "1"
}

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "adjustId": "1794318409405331968"
    },
    "retExtInfo": {},
    "time": 1728635422833
}

Get Loan LTV Adjustment History
Query for your LTV adjustment history.

Permission: "Spot trade"

info
Support querying last 6 months adjustment transactions
Only the ltv adjustment transactions launched by the user can be queried
HTTP Request
GET /v5/crypto-loan/adjustment-history

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID
adjustId	false	string	Collateral adjustment transaction ID
collateralCurrency	false	string	Collateral coin name
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> collateralCurrency	string	Collateral coin
> orderId	string	Loan order ID
> adjustId	string	Collateral adjustment transaction ID
> adjustTime	string	Adjust timestamp
> preLTV	string	LTV before the adjustment
> afterLTV	string	LTV after the adjustment
> direction	integer	The direction of adjustment, 0: add collateral; 1: reduce collateral
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan/adjustment-history?adjustId=1794318409405331968 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728635871668
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "request.success",
    "result": {
        "list": [
            {
                "adjustId": "1794318409405331968",
                "adjustTime": "1728635422814",
                "afterLTV": "0.7164",
                "amount": "0.001",
                "collateralCurrency": "BTC",
                "direction": 1,
                "orderId": "1794267532472646144",
                "preLTV": "0.6546"
            }
        ],
        "nextPageCursor": "1844656778923966466"
    },
    "retExtInfo": {},
    "time": 1728635873329
}

Get Product Info
tip
When queried without an API key, this endpoint returns public product data
If your UID is bound with an OTC loan, then you can get your private product data by calling with your API key
If your UID is not bound with an OTC loan but you passed your API key, this endpoint returns public product data
HTTP Request
GET /v5/ins-loan/product-infos

Request Parameters
Parameter	Required	Type	Comments
productId	false	string	Product ID. If not passed, returns all products
Response Parameters
Parameter	Type	Comments
marginProductInfo	array	Object
> productId	string	Product ID
> leverage	string	The maximum leverage for this loan product
> supportSpot	integer	Whether or not Spot is supported. 0:false; 1:true
> supportContract	integer	Whether USDT Perpetuals are supported. 0:false; 1:true
> supportMarginTrading	integer	Whether or not Spot margin trading is supported. 0:false; 1:true
> deferredLiquidationLine	string	Line for deferred liquidation
> deferredLiquidationTime	string	Time for deferred liquidation
> withdrawLine	string	Restrict line for withdrawal
> transferLine	string	Restrict line for transfer
> spotBuyLine	string	Restrict line for Spot buy
> spotSellLine	string	Restrict line for Spot trading
> contractOpenLine	string	Restrict line for USDT Perpetual open position
> liquidationLine	string	Line for liquidation
> stopLiquidationLine	string	Line for stop liquidation
> contractLeverage	string	The allowed default leverage for USDT Perpetual
> transferRatio	string	The transfer ratio for loan funds to transfer from Spot wallet to Contract wallet
> spotSymbols	array	The whitelist of spot trading pairs
If supportSpot="0", then it returns "[]"
If empty array, then you can trade any symbols
If not empty, then you can only trade listed symbols
> contractSymbols	array	The whitelist of contract trading pairs
If supportContract="0", then it returns "[]"
If empty array, then you can trade any symbols
If not empty, then you can only trade listed symbols
> supportUSDCContract	integer	Whether or not USDC contracts are supported. '0':false; '1':true
> supportUSDCOptions	integer	Whether or not Options are supported. '0':false; '1':true
> USDTPerpetualOpenLine	string	Restrict line to open USDT Perpetual position
> USDCContractOpenLine	string	Restrict line to open USDC Contract position
> USDCOptionsOpenLine	string	Restrict line to open Option position
> USDTPerpetualCloseLine	string	Restrict line to trade USDT Perpetual
> USDCContractCloseLine	string	Restrict line to trade USDC Contract
> USDCOptionsCloseLine	string	Restrict line to trade Option
> USDCContractSymbols	array	The whitelist of USDC contract trading pairs
If supportContract="0", then it returns "[]"
If no whitelist symbols, it is [], and you can trade any
If supportUSDCContract="0", it is []
> USDCOptionsSymbols	array	The whitelist of Option symbols
If supportContract="0", then it returns "[]"
If no whitelisted, it is [], and you can trade any
If supportUSDCOptions="0", it is []
> marginLeverage	string	The allowable maximum leverage for Spot margin trading. If supportMarginTrading=0, then it returns ""
> USDTPerpetualLeverage	array	Object
If supportContract="0", it is []
If no whitelist USDT perp symbols, it returns all trading symbols and leverage by default
If there are whitelist symbols, it return those whitelist data
>> symbol	string	Symbol name
>> leverage	string	Maximum leverage
> USDCContractLeverage	array	Object
If supportUSDCContract="0", it is []
If no whitelist USDC contract symbols, it returns all trading symbols and leverage by default
If there are whitelist symbols, it return those whitelist data
>> symbol	string	Symbol name
>> leverage	string	Maximum leverage
Request Example
HTTP
 
  
GET /v5/ins-loan/product-infos?productId=91 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "marginProductInfo": [
            {
                "productId": "91",
                "leverage": "4.00000000",
                "supportSpot": 1,
                "supportContract": 0,
                "withdrawLine": "",
                "transferLine": "",
                "spotBuyLine": "",
                "spotSellLine": "",
                "contractOpenLine": "",
                "liquidationLine": "0.75",
                "stopLiquidationLine": "0.35000000",
                "contractLeverage": "0",
                "transferRatio": "0",
                "spotSymbols": [],
                "contractSymbols": [],
                "supportUSDCContract": 0,
                "supportUSDCOptions": 0,
                "USDTPerpetualOpenLine": "",
                "USDCContractOpenLine": "",
                "USDCOptionsOpenLine": "",
                "USDTPerpetualCloseLine": "",
                "USDCContractCloseLine": "",
                "USDCOptionsCloseLine": "",
                "USDCContractSymbols": [],
                "USDCOptionsSymbols": [],
                "marginLeverage": "0",
                "USDTPerpetualLeverage": [],
                "USDCContractLeverage": [],
                "deferredLiquidationLine":"",
                "deferredLiquidationTime":"",
            }
        ]
    },
    "retExtInfo": {},
    "time": 1689747746332
}

Get Margin Coin Info
tip
When queried without an API key, this endpoint returns public margin data
If your UID is bound with an OTC loan, then you can get your private margin data by calling with your API key
If your UID is not bound with an OTC loan but you passed your API key, this endpoint returns public margin data
HTTP Request
GET /v5/ins-loan/ensure-tokens-convert

Request Parameters
Parameter	Required	Type	Comments
productId	false	string	Product ID. If not passed, returns all margin products. For spot, it returns coins with a convertRatio greater than 0.
Response Parameters
Parameter	Type	Comments
marginToken	array	Object
> productId	string	Product Id
> tokenInfo	array	Spot margin coin
>> token	string	Margin coin
>> convertRatioList	array	Margin coin convert ratio List
>>> ladder	string	ladder
>>> convertRatio	string	Margin coin convert ratio
Request Example
HTTP
 
  
GET /v5/ins-loan/ensure-tokens-convert HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "marginToken": [
            {
                "productId": "81",
                "tokenInfo": [
                    {
                        "token": "USDT",
                        "convertRatioList": [
                            {
                                "ladder": "0-500",
                                "convertRatio": "0.95"
                            },
                            {
                                "ladder": "500-1000",
                                "convertRatio": "0.9"
                            },
                            {
                                "ladder": "1000-2000",
                                "convertRatio": "0.8"
                            },
                            {
                                "ladder": "2000-4000",
                                "convertRatio": "0.7"
                            },
                            {
                                "ladder": "4000-99999999999",
                                "convertRatio": "0.6"
                            }
                        ]
                    }
                  ...
                ]
            },
            {
                "productId": "82",
                "tokenInfo": [
                    ...
                    {
                        "token": "USDT",
                        "convertRatioList": [
                            {
                                "ladder": "0-1000",
                                "convertRatio": "0.7"
                            },
                            {
                                "ladder": "1000-2000",
                                "convertRatio": "0.65"
                            },
                            {
                                "ladder": "2000-99999999999",
                                "convertRatio": "0.6"
                            }
                        ]
                    }
                ]
            },
            {
                "productId": "84",
                "tokenInfo": [
                    ...
                    {
                        "token": "BTC",
                        "convertRatioList": [
                            {
                                "ladder": "0-1000",
                                "convertRatio": "1"
                            },
                            {
                                "ladder": "1000-5000",
                                "convertRatio": "0.9"
                            },
                            {
                                "ladder": "5000-99999999999",
                                "convertRatio": "0.55"
                            }
                        ]
                    }
                ]
            }
        ]
    },
    "retExtInfo": {},
    "time": 1683276016497
}

Get Loan Orders
Get up to 2 years worth of historical loan orders.

HTTP Request
GET /v5/ins-loan/loan-order

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID. If not passed, returns all orders sorted by loanTime in descending order
startTime	false	integer	The start timestamp (ms)
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size. [1, 100], Default: 10
Response Parameters
Parameter	Type	Comments
loanInfo	array	Object
> orderId	string	Loan order ID
> orderProductId	string	Product ID
> parentUid	string	The designated UID that was used to bind with the INS loan
> loanTime	string	Loan timestamp, in milliseconds
> loanCoin	string	Loan coin
> loanAmount	string	Loan amount
> unpaidAmount	string	Unpaid principal
> unpaidInterest	string	Unpaid interest
> repaidAmount	string	Repaid principal
> repaidInterest	string	Repaid interest
> interestRate	string	Daily interest rate
> status	string	1：outstanding; 2：paid off
> leverage	string	The maximum leverage for this loan product
> supportSpot	string	Whether to support spot. 0:false; 1:true
> supportContract	string	Whether to support contract . 0:false; 1:true
> withdrawLine	string	Restrict line for withdrawal
> transferLine	string	Restrict line for transfer
> spotBuyLine	string	Restrict line for SPOT buy
> spotSellLine	string	Restrict line for SPOT sell
> contractOpenLine	string	Restrict line for USDT Perpetual open position
> deferredLiquidationLine	string	Line for deferred liquidation
> deferredLiquidationTime	string	Time for deferred liquidation
> reserveToken	string	Reserve token
> reserveQuantity	string	Reserve token qty
> liquidationLine	string	Line for liquidation
> stopLiquidationLine	string	Line for stop liquidation
> contractLeverage	string	The allowed default leverage for USDT Perpetual
> transferRatio	string	The transfer ratio for loan funds to transfer from Spot wallet to Contract wallet
> spotSymbols	array	The whitelist of spot trading pairs. If there is no whitelist, then "[]"
> contractSymbols	array	The whitelist of contract trading pairs
If supportContract="0", then this is "[]"
If there is no whitelist, this is "[]"
> supportUSDCContract	string	Whether to support USDC contract. "0":false; "1":true
> supportUSDCOptions	string	Whether to support Option. "0":false; "1":true
> supportMarginTrading	string	Whether to support Spot margin trading. "0":false; "1":true
> USDTPerpetualOpenLine	string	Restrict line to open USDT Perpetual position
> USDCContractOpenLine	string	Restrict line to open USDC Contract position
> USDCOptionsOpenLine	string	Restrict line to open Option position
> USDTPerpetualCloseLine	string	Restrict line to trade USDT Perpetual position
> USDCContractCloseLine	string	Restrict line to trade USDC Contract position
> USDCOptionsCloseLine	string	Restrict line to trade Option position
> USDCContractSymbols	array	The whitelist of USDC contract trading pairs
If no whitelist symbols, it is [], and you can trade any
If supportUSDCContract="0", it is []
> USDCOptionsSymbols	array	The whitelist of Option symbols
If no whitelisted, it is [], and you can trade any
If supportUSDCOptions="0", it is []
> marginLeverage	string	The allowable maximum leverage for Spot margin
> USDTPerpetualLeverage	array	Object
If supportContract="0", it is []
If no whitelist USDT perp symbols, it returns all trading symbols and leverage by default
If there are whitelist symbols, it return those whitelist data
>> symbol	string	Symbol name
>> leverage	string	Maximum leverage
> USDCContractLeverage	array	Object
If supportUSDCContract="0", it is []
If no whitelist USDC contract symbols, it returns all trading symbols and leverage by default
If there are whitelist symbols, it return those whitelist data
>> symbol	string	Symbol name
>> leverage	string	Maximum leverage
Request Example
HTTP
 
  
GET /v5/ins-loan/loan-order HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1678687874060
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "loanInfo": [
            {
                "orderId": "1468005106166530304",
                "orderProductId": "96",
                "parentUid": "1631521",
                "loanTime": "1689735916000",
                "loanCoin": "USDT",
                "loanAmount": "204",
                "unpaidAmount": "52.07924201",
                "unpaidInterest": "0",
                "repaidAmount": "151.92075799",
                "repaidInterest": "0",
                "interestRate": "0.00019178",
                "status": "1",
                "leverage": "4",
                "supportSpot": "1",
                "supportContract": "1",
                "withdrawLine": "",
                "transferLine": "",
                "spotBuyLine": "0.71",
                "spotSellLine": "0.71",
                "contractOpenLine": "0.71",
                "liquidationLine": "0.75",
                "stopLiquidationLine": "0.35000000",
                "contractLeverage": "7",
                "transferRatio": "1",
                "spotSymbols": [],
                "contractSymbols": [],
                "supportUSDCContract": "1",
                "supportUSDCOptions": "1",
                "USDTPerpetualOpenLine": "0.71",
                "USDCContractOpenLine": "0.71",
                "USDCOptionsOpenLine": "0.71",
                "USDTPerpetualCloseLine": "0.71",
                "USDCContractCloseLine": "0.71",
                "USDCOptionsCloseLine": "0.71",
                "USDCContractSymbols": [],
                "USDCOptionsSymbols": [],
                "deferredLiquidationLine":"",
                "deferredLiquidationTime":"",
                "marginLeverage": "4",
                "USDTPerpetualLeverage": [
                    {
                        "symbol": "SUSHIUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "INJUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "RDNTUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "ZRXUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "HIGHUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "WAVESUSDT",
                        "leverage": "7"
                    },
                    ...
                    {
                        "symbol": "ACHUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "SUNUSDT",
                        "leverage": "7"
                    }
                ],
                "USDCContractLeverage": [
                    {
                        "symbol": "BTCPERP",
                        "leverage": "8"
                    },
                    {
                        "symbol": "BTC-Futures",
                        "leverage": "8"
                    },
                    ...
                    {
                        "symbol": "ETH-Futures",
                        "leverage": "8"
                    },
                    {
                        "symbol": "SOLPERP",
                        "leverage": "8"
                    },
                    {
                        "symbol": "ETHPERP",
                        "leverage": "8"
                    }
                ]
            }
        ]
    },
    "retExtInfo": {},
    "time": 1689745773187
}

Get Repayment Orders
Get a list of your loan repayment orders (orders which repaid the loan).

tip
Get the past 2 years data by default
Get up to the past 2 years of data
HTTP Request
GET /v5/ins-loan/repaid-history

Request Parameters
Parameter	Required	Type	Comments
startTime	false	integer	The start timestamp (ms)
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size. [1, 100]. Default: 100
Response Parameters
Parameter	Type	Comments
repayInfo	array	Object
> repayOrderId	string	Repaid order ID
> repaidTime	string	Repaid timestamp (ms)
> token	string	Repaid coin
> quantity	string	Repaid principle
> interest	string	Repaid interest
> businessType	string	Repaid type. 1：normal repayment; 2：repaid by liquidation
> status	string	1：success; 2：fail
Request Example
HTTP
 
  
GET /v5/ins-loan/repaid-history HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN-TYPE: 2
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1678687944725
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "repayInfo": [
            {
                "repayOrderId": "8189",
                "repaidTime": "1663126393000",
                "token": "USDT",
                "quantity": "30000",
                "interest": "0",
                "businessType": "1",
                "status": "1"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1669366648366
}

Get LTV
Get your loan-to-value (LTV) ratio.

HTTP Request
GET /v5/ins-loan/ltv-convert

Request Parameters
None

Response Parameters
Parameter	Type	Comments
ltvInfo	array	Object
> ltv	string	Risk rate
ltv is calculated in real time
If you have an INS loan, it is highly recommended to query this data every second. Liquidation occurs when it reachs 0.9 (90%)
> rst	string	Remaining liquidation time (UTC time in seconds). When it is not triggered, it is displayed as an empty string.
> parentUid	string	The designated Risk Unit ID that was used to bind with the INS loan
> subAccountUids	array	Bound user ID
> unpaidAmount	string	Total debt(USDT)
> unpaidInfo	array	Debt details
>> token	string	coin
>> unpaidQty	string	Unpaid principle
>> unpaidInterest	string	Useless field, please ignore this for now
> balance	string	Total asset (margin coins converted to USDT). Please read here to understand the calculation
> balanceInfo	array	Asset details
>> token	string	Margin coin
>> price	string	Margin coin price
>> qty	string	Margin coin quantity
>> convertedAmount	string	Margin conversion amount
Request Example
HTTP
 
  
GET /v5/ins-loan/ltv-convert HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1686638165351
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "ltvInfo": [
            {
                "ltv": "0.75",
                "rst": "",
                "parentUid": "xxxxx",
                "subAccountUids": [
                    "60568258"
                ],
                "unpaidAmount": "30",
                "unpaidInfo": [
                    {
                        "token": "USDT",
                        "unpaidQty": "30",
                        "unpaidInterest": "0"
                    }
                ],
                "balance": "40",
                "balanceInfo": [
                    {
                        "token": "USDT",
                        "price": "1",
                        "qty": "40",
                        "convertedAmount": "40"
                    }
                ]
            }
        ]
    },
    "retExtInfo": {},
    "time": 1686638166323
}

Bind Or Unbind UID
For the institutional loan product, you can bind new UIDs to the risk unit or unbind UID from the risk unit.

info
The risk unit designated UID cannot be unbound.
The UID you want to bind must be upgraded to UTA Pro.
HTTP Request
POST /v5/ins-loan/association-uid

Request Parameters
Parameter	Required	Type	Comments
uid	true	string	UID
Bind
a) the key used must be from one of UIDs in the risk unit;
b) input UID must not have an INS loan
Unbind
a) the key used must be from one of UIDs in the risk unit;
b) input UID cannot be the same as the UID used to access the API
operate	true	string	0: bind, 1: unbind
Response Parameters
Parameter	Type	Comments
uid	string	UID
operate	string	0: bind, 1: unbind
Request Example
HTTP
 
  
POST /v5/ins-loan/association-uid HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1699257853101
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
Content-Type: application/json
Content-Length: 43

{
    "uid": "592324",
    "operate": "0"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "uid": "592324",
        "operate": "0"
    },
    "retExtInfo": {},
    "time": 1699257746135
}

Get Earning
info
Use exchange broker master account to query
The data can support up to past 1 months until T-1. To extract data from over a month a  , please contact your Relationship Manager
begin & end are either entered at the same time or not entered, and latest 7 days data are returned by default
API rate limit: 10 req / sec

HTTP Request
GET /v5/broker/earnings-info

Request Parameters
Parameter	Required	Type	Comments
bizType	false	string	Business type. SPOT, DERIVATIVES, OPTIONS, CONVERT
begin	false	string	Begin date, in the format of YYYYMMDD, e.g, 20231201, search the data from 1st Dec 2023 00:00:00 UTC (include)
end	false	string	End date, in the format of YYYYMMDD, e.g, 20231201, search the data before 2nd Dec 2023 00:00:00 UTC (exclude)
uid	false	string	
To get results for a specific subaccount: Enter the subaccount UID
To get results for all subaccounts: Leave the field empty
limit	false	integer	Limit for data size per page. [1, 1000]. Default: 1000
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
totalEarningCat	Object	Cate  ry statistics for total earning data
> spot	array	Object. Earning for Spot trading. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
> derivatives	array	Object. Earning for Derivatives trading. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
> options	array	Object. Earning for Option trading. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
> convert	array	Object. Earning for Convert trading. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
> total	array	Object. Sum earnings of all cate  ries. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
details	array	Object. Detailed trading information for each sub UID and each cate  ry
> userId	string	Sub UID
> bizType	string	Business type. SPOT, DERIVATIVES, OPTIONS, CONVERT
> symbol	string	Symbol name
> coin	string	Rebate coin name
> earning	string	Rebate amount
> markupEarning	string	Earning generated from markup fee rate
> baseFeeEarning	string	Earning generated from base fee rate
> orderId	string	Order ID
> execId	string	Trade ID
> execTime	string	Order execution timestamp (ms)
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/broker/earnings-info?begin=20231129&end=20231129&uid=117894077 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1701399431920
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: 32d2aa1bc205ddfb89849b85e2a8b7e23b1f8f69fe95d6f2cb9c87562f9086a6
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "totalEarningCat": {
            "spot": [],
            "derivatives": [
                {
                    "coin": "USDT",
                    "earning": "0.00027844"
                }
            ],
            "options": [],
            "total": [
                {
                    "coin": "USDT",
                    "earning": "0.00027844"
                }
            ]
        },
        "details": [
            {
                "userId": "117894077",
                "bizType": "DERIVATIVES",
                "symbol": "DOGEUSDT",
                "coin": "USDT",
                "earning": "0.00016166",
                "markupEarning": "0.000032332",
                "baseFeeEarning": "0.000129328",
                "orderId": "ec2132f2-a7e0-4a0c-9219-9f3cbcd8e878",
                "execId": "c8f418a0-2ccc-594f-ae72-effedf24d0c4",
                "execTime": "1701275846033"
            },
            {
                "userId": "117894077",
                "bizType": "DERIVATIVES",
                "symbol": "TRXUSDT",
                "coin": "USDT",
                "earning": "0.00011678",
                "markupEarning": "0.000023356",
                "baseFeeEarning": "0.000093424",
                "orderId": "28b29c2b-ba14-450e-9ce7-3cee0c1fa6da",
                "execId": "632c7705-7f3a-5350-b69c-d41a8b3d0697",
                "execTime": "1701245285017"
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1701398193964
}

Get Account Info
info
Use exchange broker master account to query
API rate limit: 10 req / sec

HTTP Request
GET /v5/broker/account-info

Request Parameters
None

Response Parameters
Parameter	Type	Comments
subAcctQty	string	The qty of sub account has been created
maxSubAcctQty	string	The max limit of sub account can be created
baseFeeRebateRate	Object	Rebate percentage of the base fee
> spot	string	Rebate percentage of the base fee for spot, e.g., 10.00%
> derivatives	string	Rebate percentage of the base fee for derivatives, e.g., 10.00%
markupFeeRebateRate	Object	Rebate percentage of the mark up fee
> spot	string	Rebate percentage of the mark up fee for spot, e.g., 10.00%
> derivatives	string	Rebate percentage of the mark up fee for derivatives, e.g., 10.00%
> convert	string	Rebate percentage of the mark up fee for convert, e.g., 10.00%
ts	string	System timestamp (ms)
Request Example
HTTP
 
  
GET /v5/broker/account-info HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1701399431920
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "subAcctQty": "2",
        "maxSubAcctQty": "20",
        "baseFeeRebateRate": {
            "spot": "10.0%",
            "derivatives": "10.0%"
        },
        "markupFeeRebateRate": {
            "spot": "6.00%",
            "derivatives": "9.00%",
            "convert": "3.00%",
        },
        "ts": "1701395633402"
    },
    "retExtInfo": {},
    "time": 1701395633403
}

Get Sub Account Deposit Records
Exchange broker can query subaccount's deposit records by main UID's API key without specifying uid.

API rate limit: 300 req / min

tip
endTime - startTime should be less than 30 days. Queries for the last 30 days worth of records by default.

HTTP Request
GET /v5/broker/asset/query-sub-member-deposit-record

Request Parameters
Parameter	Required	Type	Comments
id	false	string	Internal ID: Can be used to uniquely identify and filter the deposit. When combined with other parameters, this field takes the highest priority
txID	false	string	Transaction ID: Please note that data generated before Jan 1, 2024 cannot be queried using txID
subMemberId	false	string	Sub UID
coin	false	string	Coin, uppercase only
startTime	false	integer	The start timestamp (ms) Note: the query logic is actually effective based on second level
endTime	false	integer	The end timestamp (ms) Note: the query logic is actually effective based on second level
limit	false	integer	Limit for data size per page. [1, 50]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
rows	array	Object
> id	string	Unique ID
> subMemberId	string	Sub account user ID
> coin	string	Coin
> chain	string	Chain
> amount	string	Amount
> txID	string	Transaction ID
> status	integer	Deposit status
> toAddress	string	Deposit target address
> tag	string	Tag of deposit target address
> depositFee	string	Deposit fee
> successAt	string	Last updated time
> confirmations	string	Number of confirmation blocks
> txIndex	string	Transaction sequence number
> blockHash	string	Hash number on the chain
> batchReleaseLimit	string	The deposit limit for this coin in this chain. "-1" means no limit
> depositType	string	The deposit type. 0: normal deposit, 10: the deposit reaches daily deposit limit, 20: abnormal deposit
> fromAddress	string	From address of deposit, only shown when the deposit comes from on-chain and from address is unique, otherwise gives ""
> taxDepositRecordsId	string	This field is used for tax purposes by Bybit EU (Austria) users， declare tax id
> taxStatus	integer	This field is used for tax purposes by Bybit EU (Austria) users
0: No reporting required
1: Reporting pending
2: Reporting completed
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/broker/asset/query-sub-member-deposit-record?coin=USDT&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672192441294
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "rows": [],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1672192441742
}

Query Voucher Spec
HTTP Request
POST /v5/broker/award/info

Request Parameters
Parameter	Required	Type	Comments
id	true	string	Voucher ID
Response Parameters
Parameter	Type	Comments
id	string	Voucher ID
coin	string	Coin
amountUnit	string	
AWARD_AMOUNT_UNIT_USD
AWARD_AMOUNT_UNIT_COIN
productLine	string	Product line
subProductLine	string	Sub product line
totalAmount	Object	Total amount of voucher
usedAmount	string	Used amount of voucher
Request Example
HTTP
 
  
POST /v5/broker/award/info HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1726107086048
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 22

{
    "id": "80209"
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "id": "80209",
        "coin": "USDT",
        "amountUnit": "AWARD_AMOUNT_UNIT_USD",
        "productLine": "PRODUCT_LINE_CONTRACT",
        "subProductLine": "SUB_PRODUCT_LINE_CONTRACT_DEFAULT",
        "totalAmount": "10000",
        "usedAmount": "100"
    },
    "retExtInfo": {},
    "time": 1726107086313
}

Issue Voucher
HTTP Request
POST /v5/broker/award/distribute-award

Request Parameters
Parameter	Required	Type	Comments
accountId	true	string	User ID
awardId	true	string	Voucher ID
specCode	true	string	Customised unique spec code, up to 8 characters
amount	true	string	Issue amount
Spot airdrop supports up to 16 decimals
Other types supports up to 4 decimals
brokerId	true	string	Broker ID
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/broker/award/distribute-award HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1726110531734
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 128

{
    "accountId": "2846381",
    "awardId": "123456",
    "specCode": "award-001",
    "amount": "100",
    "brokerId": "v-28478"
}

Response Example
{
    "retCode": 0,
    "retMsg": ""
}

Query Issued Voucher
HTTP Request
POST /v5/broker/award/distribution-record

Request Parameters
Parameter	Required	Type	Comments
accountId	true	string	User ID
awardId	true	string	Voucher ID
specCode	true	string	Customised unique spec code, up to 8 characters
withUsedAmount	false	boolean	Whether to return the amount used by the user
true
false (default)
Response Parameters
Parameter	Type	Comments
accountId	string	User ID
awardId	string	Voucher ID
specCode	string	Spec code
amount	string	Amount of voucher
isClaimed	boolean	true, false
startAt	string	Claim start timestamp (sec)
endAt	string	Claim end timestamp (sec)
effectiveAt	string	Voucher effective timestamp (sec) after claimed
ineffectiveAt	string	Voucher inactive timestamp (sec) after claimed
usedAmount	string	Amount used by the user
Request Example
HTTP
 
  
POST /v5/broker/award/distribution-record HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1726112099846
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 111

{
    "accountId": "5714139",
    "awardId": "189528",
    "specCode": "demo000",
    "withUsedAmount": false
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "accountId": "5714139",
        "awardId": "189528",
        "specCode": "demo000",
        "amount": "1",
        "isClaimed": true,
        "startAt": "1725926400",
        "endAt": "1733788800",
        "effectiveAt": "1726531200",
        "ineffectiveAt": "1733817600",
        "usedAmount": "",
    }
}

Get Product Info
info
Does not need authentication.

Bybit Saving FAQ

HTTP Request
GET /v5/earn/product

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	FlexibleSaving,OnChain
Remarks: currently, only flexible savings and on chain is supported
coin	false	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
list	array	Object
> cate  ry	string	FlexibleSaving,OnChain
> estimateApr	string	Estimated APR, e.g., 3%, 4.25%
Remarks: 1)The Est. APR provides a dynamic preview of your potential returns, updated every 10 minutes in response to market conditions.
2) Please note that this is an estimate and may differ from the actual APR you will receive.
3) Platform Reward APRs are not shown
> coin	string	Coin name
> minStakeAmount	string	Minimum stake amount
> maxStakeAmount	string	Maximum stake amount
> precision	string	Amount precision
> productId	string	Product ID
> status	string	Available, NotAvailable
> bonusEvents	Array	Bonus
>> apr	string	Yesterday's Rewards APR
>> coin	string	Reward coin
>> announcement	string	Announcement link
> minRedeemAmount	string	Minimum redemption amount. Only has value in Onchain LST mode
> maxRedeemAmount	string	Maximum redemption amount. Only has value in Onchain LST mode
> duration	string	Fixed,Flexible. Product Type
> term	int	Unit: Day. Only when duration = Fixed for OnChain
> swapCoin	string	swap coin. Only has value in Onchain LST mode
> swapCoinPrecision	string	swap coin precision. Only has value in Onchain LST mode
> stakeExchangeRate	string	Estimated stake exchange rate. Only has value in Onchain LST mode
> redeemExchangeRate	string	Estimated redeem exchange rate. Only has value in Onchain LST mode
> rewardDistributionType	string	Simple: Simple interest, Compound: Compound interest, Other: LST. Only has value for Onchain
> rewardIntervalMinute	int	Frequency of reward distribution (minutes)
> redeemProcessingMinute	string	Estimated redemption minutes
> stakeTime	string	Staking on-chain time, in milliseconds
> interestCalculationTime	string	Interest accrual time, in milliseconds
Request Example
HTTP
 
  
GET /v5/earn/product?cate  ry=FlexibleSaving&coin=BTC HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "list": [
            {
                "cate  ry": "FlexibleSaving",
                "estimateApr": "3%",
                "coin": "BTC",
                "minStakeAmount": "0.001",
                "maxStakeAmount": "10",
                "precision": "8",
                "productId": "430",
                "status": "Available",
                "bonusEvents": [],
                "minRedeemAmount": "",
                "maxRedeemAmount": "",
                "duration": "",
                "term": 0,
                "swapCoin": "",
                "swapCoinPrecision": "",
                "stakeExchangeRate": "",
                "redeemExchangeRate": "",
                "rewardDistributionType": "",
                "rewardIntervalMinute": 0,
                "redeemProcessingMinute": 0,
                "stakeTime": "",
                "interestCalculationTime": ""
            }
        ]
    },
    "retExtInfo": {},
    "time": 1739935669110
}

Stake / Redeem
info
API key needs "Earn" permission

note
In times of high demand for loans in the market for a specific cryptocurrency, the redemption of the principal may encounter delays and is expected to be processed within 48 hours. The redemption of on-chain products may take up to a few days to complete. Once the redemption request is initiated, it cannot be cancelled.

HTTP Request
POST /v5/earn/place-order

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	FlexibleSaving,OnChain
Remarks: currently, only flexible savings and on chain is supported
orderType	true	string	Stake, Redeem
accountType	true	string	FUND, UNIFIED. Onchain only supports FUND
amount	true	string	
Stake amount needs to satisfy minStake and maxStake
Both stake and redeem amount need to satify precision requirement
coin	true	string	Coin name
productId	true	string	Product ID
orderLinkId	true	string	Customised order ID, used to prevent from replay
support up to 36 characters
The same orderLinkId can't be used in 30 mins
redeemPositionId	false	string	The position ID that the user needs to redeem. Only is required in Onchain non-LST mode
Response Parameters
Parameter	Type	Comments
orderId	string	Order ID
orderLinkId	string	Order link ID
Request Example
HTTP
 
  
POST /v5/earn/place-order HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1739936605822
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 190

{
    "cate  ry": "FlexibleSaving",
    "orderType": "Redeem",
    "accountType": "FUND",
    "amount": "0.35",
    "coin": "BTC",
    "productId": "430",
    "orderLinkId": "btc-earn-001"
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "orderId": "0572b030-6a0b-423f-88c4-b6ce31c0c82d",
        "orderLinkId": "btc-earn-001"
    },
    "retExtInfo": {},
    "time": 1739936607117
}

Get Stake/Redeem Order History
info
API key needs "Earn" permission

HTTP Request
GET /v5/earn/order

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	FlexibleSaving,OnChain
Remarks: currently, only flexible savings and OnChain is supported
orderId	false	string	Order ID
either orderId or orderLinkId is required
if both are passed, make sure they're matched, otherwise returning empty result
orderLinkId	false	string	Order link ID
Remarks: Always return the latest one if order link id is ever reused when querying by orderLinkId only
Response Parameters
Parameter	Type	Comments
list	array	Object
> coin	string	Coin name
> orderValue	string	amount
> orderType	string	Redeem, Stake
> orderId	string	Order ID
> orderLinkId	string	Order link ID
> status	string	Order status Success, Fail, Pending
> createdAt	string	Order created time, in milliseconds
> productId	string	Product ID
> updatedAt	string	Order updated time, in milliseconds
> swapOrderValue	string	Swap order value. Only for LST Onchain.
> estimateRedeemTime	string	Estimate redeem time, in milliseconds. Only for Onchain
> estimateStakeTime	string	Estimate stake time, in milliseconds. Only for Onchain
Request Example
HTTP
 
  
GET /v5/earn/order?orderId=9640dc23-df1a-448a-ad24-e1a48028a51f&cate  ry=OnChain HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1739937044221
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "list": [
            {
                "coin": "BTC",
                "orderValue": "1",
                "orderType": "Stake",
                "orderId": "9640dc23-df1a-448a-ad24-e1a48028a51f",
                "orderLinkId": "cjm2",
                "status": "Success",
                "createdAt": "1744166831000",
                "productId": "8",
                "updatedAt": "1744166832000",
                "swapOrderValue": "",
                "estimateRedeemTime": "",
                "estimateStakeTime": ""
            }
        ]
    },
    "retExtInfo": {},
    "time": 1739937045520
}

Get Staked Position
info
API key needs "Earn" permission

note
For Flexible Saving, fully redeemed position is also returned in the response For Onchain, only active position will be returned in the response

HTTP Request
GET /v5/earn/position

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	FlexibleSaving,OnChain
productId	false	string	Product ID
coin	false	string	Coin name
Response Parameters
Parameter	Type	Comments
list	array	Object
> coin	string	Coin name
> productId	string	Product ID
> amount	string	Total staked amount
> totalPnl	string	Return the profit of the current position. Only has value in Onchain non-LST mode
> claimableYield	string	Yield accrues on an hourly basis and is distributed at 00:30 UTC daily. If you unstake your assets before yield distribution, any undistributed yield will be credited to your account along with your principal. Onchain products do not return values
> id	string	Position Id. Only for Onchain
> status	string	Processing,Active. Only for Onchain
> orderId	string	Order Id. Only for Onchain
> estimateRedeemTime	string	Estimate redeem time, in milliseconds. Only for Onchain
> estimateStakeTime	string	Estimate stake time, in milliseconds. Only for Onchain
> estimateInterestCalculationTime	string	Estimated Interest accrual time, in milliseconds. Only for Onchain
> settlementTime	string	Settlement time, in milliseconds. Only has value for Onchain Fixed product
Request Example
HTTP
 
  
GET /v5/earn/position?cate  ry=FlexibleSaving&coin=USDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1739944576277
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "list": [
            {
                "coin": "BTC",
                "productId": "8",
                "amount": "0.1",
                "totalPnl": "0.000027397260273973",
                "claimableYield": "0",
                "id": "326",
                "status": "Active",
                "orderId": "1a5a8945-e042-4dd5-a93f-c0f0577377ad",
                "estimateRedeemTime": "",
                "estimateStakeTime": "",
                "estimateInterestCalculationTime": "1744243200000",
                "settlementTime": "1744675200000"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1739944577575
}

Connect
WebSocket public stream:

Mainnet:
Spot: wss://stream.bybit.com/v5/public/spot
USDT, USDC perpetual & USDT Futures: wss://stream.bybit.com/v5/public/linear
Inverse contract: wss://stream.bybit.com/v5/public/inverse
Spread trading: wss://stream.bybit.com/v5/public/spread
USDT/USDC Options: wss://stream.bybit.com/v5/public/option

Testnet:
Spot: wss://stream-testnet.bybit.com/v5/public/spot
USDT,USDC perpetual & USDT Futures: wss://stream-testnet.bybit.com/v5/public/linear
Inverse contract: wss://stream-testnet.bybit.com/v5/public/inverse
Spread trading: wss://stream-testnet.bybit.com/v5/public/spread
USDT/USDC Options: wss://stream-testnet.bybit.com/v5/public/option

WebSocket private stream:

Mainnet:
wss://stream.bybit.com/v5/private

Testnet:
wss://stream-testnet.bybit.com/v5/private

WebSocket Order Entry:

Mainnet:
wss://stream.bybit.com/v5/trade (Spread trading is not supported)

Testnet:
wss://stream-testnet.bybit.com/v5/trade (Spread trading is not supported)

WebSocket GET System Status:

Mainnet:
wss://stream.bybit.com/v5/public/misc/status

Testnet:
wss://stream-testnet.bybit.com/v5/public/misc/status

info
If your account is registered from www.bybit-tr.com, please use stream.bybit-tr.com for mainnet access
If your account is registered from www.bybit.kz, please use stream.bybit.kz for mainnet access
If your account is registered from www.bybitgeorgia.ge, please use stream.bybitgeorgia.ge for mainnet access
Customise Private Connection Alive Time
For private stream and order entry, you can customise alive duration by adding a param max_active_time, the lowest value is 30s (30 seconds), the highest value is 600s (10 minutes). You can also pass 1m, 2m etc when you try to configure by minute level. e.g., wss://stream-testnet.bybit.com/v5/private?max_active_time=1m.

In general, if there is no "ping-pong" and no stream data sent from server end, the connection will be cut off after 10 minutes. When you have a particular need, you can configure connection alive time by max_active_time.

Since ticker scans every 30s, so it is not fully exact, i.e., if you configure 45s, and your last update or ping-pong is occurred on 2023-08-15 17:27:23, your disconnection time maybe happened on 2023-08-15 17:28:15

Authentication
info
Public topics do not require authentication. The following section applies to private topics only.
Apply for authentication when establishing a connection.

Note: if you're using pybit, bybit-api, or another high-level library, you can ignore this code - as authentication is handled for you.

{
    "req_id": "10001", // optional
    "op": "auth",
    "args": [
        "api_key",
        1662350400000, // expires; is greater than your current timestamp
        "signature"
    ]
}

# based on: https://github.com/bybit-exchange/pybit/blob/master/pybit/_http_manager.py

import hmac
import json
import time
import websocket

api_key = ""
api_secret = ""

# Generate expires.
expires = int((time.time() + 1) * 1000)

# Generate signature.
signature = str(hmac.new(
    bytes(api_secret, "utf-8"),
    bytes(f"GET/realtime{expires}", "utf-8"), digestmod="sha256"
).hexdigest())

ws = websocket.WebSocketApp(
    url=url,
    ...
)

# Authenticate with API.
ws.send(
    json.dumps({
        "op": "auth",
        "args": [api_key, expires, signature]
    })
)

Successful authentication sample response

{
    "success": true,
    "ret_msg": "",
    "op": "auth",
    "conn_id": "cejreaspqfh3sjdnldmg-p"
}

note
Example signature al  rithms can be found here.

caution
Due to network complexity, your may get disconnected at any time. Please follow the instructions below to ensure that you receive WebSocket messages on time:

Keep connection alive by sending the heartbeat packet
Reconnect as soon as possible if disconnected
IP Limits
Do not frequently connect and disconnect the connection.
Do not build over 500 connections in 5 minutes. This is counted per WebSocket domain.
Public channel - Args limits
Regardless of Perpetual, Futures, Options or Spot, for one public connection, you cannot have length of "args" array over 21,000 characters.

Spot can input up to 10 args for each subscription request sent to one connection
Options can input up to 2000 args for a single connection
No args limit for Futures and Spread for now
How to Send the Heartbeat Packet
How to Send

// req_id is a customised ID, which is optional
ws.send(JSON.stringify({"req_id": "100001", "op": "ping"}));

Pong message example of public channels

Spot
Linear/Inverse
Option/Spread
{
    "success": true,
    "ret_msg": "pong",
    "conn_id": "0970e817-426e-429a-a679-ff7f55e0b16a",
    "op": "ping"
}

Pong message example of private channels

{
    "req_id": "test",
    "op": "pong",
    "args": [
        "1675418560633"
    ],
    "conn_id": "cfcb4ocsvfriu23r3er0-1b"
}

caution
To avoid network or program issues, we recommend that you send the ping heartbeat packet every 20 seconds to maintain the WebSocket connection.

How to Subscribe to Topics
Understanding WebSocket Filters
How to subscribe with a filter

// Subscribing level 1 orderbook
{
    "req_id": "test", // optional
    "op": "subscribe",
    "args": [
        "orderbook.1.BTCUSDT"
    ]
}

Subscribing with multiple symbols and topics is supported.

{
    "req_id": "test", // optional
    "op": "subscribe",
    "args": [
        "orderbook.1.BTCUSDT",
        "publicTrade.BTCUSDT",
        "orderbook.1.ETHUSDT"
    ]
}

Understanding WebSocket Filters: Unsubscription
You can dynamically subscribe and unsubscribe from topics without unsubscribing from the WebSocket like so:

{
    "op": "unsubscribe",
    "args": [
        "publicTrade.ETHUSD"
    ],
    "req_id": "customised_id"
}

Understanding the Subscription Response
Topic subscription response message example

Private
{
    "success": true,
    "ret_msg": "",
    "op": "subscribe",
    "conn_id": "cejreassvfrsfvb9v1a0-2m"
}

Public Spot
{
    "success": true,
    "ret_msg": "subscribe",
    "conn_id": "2324d924-aa4d-45b0-a858-7b8be29ab52b",
    "req_id": "10001",
    "op": "subscribe"
}

Linear/Inverse
{
    "success": true,
    "ret_msg": "",
    "conn_id": "3cd84cb1-4d06-4a05-930a-2efe5fc70f0f",
    "req_id": "",
    "op": "subscribe"
}

Option/Spread
{
    "success": true,
    "conn_id": "aa01fbfffe80af37-00000001-000b37b9-7147f432704fd28c-00e1c172",
    "data": {
    "failTopics": [],
    "successTopics": [
        "orderbook.100.BTC-6JAN23-18000-C"
    ]
},
    "type": "COMMAND_RESP"
}

Orderbook
Subscribe to the spread orderbook stream.

Depths
Level 25 data, push frequency: 20ms

Topic:
orderbook.{depth}.{symbol} e.g., orderbook.25.SOLUSDT_SOL/USDT

Process snapshot/delta
To process snapshot and delta messages, please follow these rules:

Once you have subscribed successfully, you will receive a snapshot. The WebSocket will keep pushing delta messages every time the orderbook changes. If you receive a new snapshot message, you will have to reset your local orderbook. If there is a problem on Bybit's end, a snapshot will be re-sent, which is guaranteed to contain the latest data.

To apply delta updates:

If you receive an amount that is 0, delete the entry
If you receive an amount that does not exist, insert it
If the entry exists, you simply update the value
See working code examples of this logic in the FAQ.

Response Parameters
Parameter	Type	Comments
topic	string	Topic name
type	string	Data type. snapshot,delta
ts	number	The timestamp (ms) that the system generates the data
data	map	Object
> s	string	Symbol name
> b	array	Bids. For snapshot stream. Sorted by price in descending order
>> b[0]	string	Bid price
>> b[1]	string	Bid size
The delta data has size=0, which means that all quotations for this price have been filled or cancelled
> a	array	Asks. For snapshot stream. Sorted by price in ascending order
>> a[0]	string	Ask price
>> a[1]	string	Ask size
The delta data has size=0, which means that all quotations for this price have been filled or cancelled
> u	integer	Update ID
Occasionally, you'll receive "u"=1, which is a snapshot data due to the restart of the service. So please overwrite your local orderbook
> seq	integer	Cross sequence
cts	number	The timestamp from the matching engine when this orderbook data is produced. It can be correlated with T from public trade channel
Subscribe Example
{
    "op": "subscribe",
    "args": ["orderbook.25.SOLUSDT_SOL/USDT"]
}

Event Example
{
    "topic": "orderbook.25.SOLUSDT_SOL/USDT",
    "ts": 1744165512257,
    "type": "delta",
    "data": {
        "s": "SOLUSDT_SOL/USDT",
        "b": [],
        "a": [
            [
                "22.3755",
                "4.7"
            ]
        ],
        "u": 64892,
        "seq": 299084
    },
    "cts": 1744165512234
}


U
                "autoRepay": "true",
                "borrowCurrency": "ETH",
                "borrowTime": "1752633756068",
                "interestPaid": "0.002531506849315069",
                "loanId": "571",
                "orderId": "13007",
                "repaymentTime": "1755225756068",
                "residualPenaltyInterest": "0",
                "residualPrincipal": "1.4",
                "status": 1,
                "term": "30"
            },
            {
                "annualRate": "0.022",
                "autoRepay": "true",
                "borrowCurrency": "ETH",
                "borrowTime": "1752633696068",
                "interestPaid": "0.00018082191780822",
                "loanId": "570",
                "orderId": "13007",
                "repaymentTime": "1755225696068",
                "residualPenaltyInterest": "0",
                "residualPrincipal": "0.1",
                "status": 1,
                "term": "30"
            }
        ],
        "nextPageCursor": "568"
    },
    "retExtInfo": {},
    "time": 1752652692603
}

Get Supply Contract Info
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-fixed/supply-contract-info

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Supply order ID
supplyId	false	string	Supply contract ID
supplyCurrency	false	string	Supply coin name
term	false	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> annualRate	string	Annual rate for the supply
> supplyCurrency	string	Supply coin
> supplyTime	string	Supply timestamp
> supplyAmount	string	Supply amount
> interestPaid	string	Paid interest
> supplyId	string	Supply contract ID
> orderId	string	Supply order ID
> redemptionTime	string	Planned time to redeem
> penaltyInterest	string	Overdue interest
> actualRedemptionTime	string	Actual time to redeem
> status	integer	Supply contract status 1: Supplying; 2: Redeemed
> term	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/supply-contract-info?supplyCurrency=USDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752654376532
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "actualRedemptionTime": "1753087596082",
                "annualRate": "0.01",
                "interest": "0.13041095890410959",
                "orderId": "13564",
                "penaltyInterest": "0",
                "redemptionTime": "1753087596082",
                "status": 1,
                "supplyAmount": "800",
                "supplyCurrency": "USDT",
                "supplyId": "567",
                "supplyTime": "1752482796082",
                "term": "7"
            }
        ],
        "nextPageCursor": "567"
    },
    "retExtInfo": {},
    "time": 1752654377461
}

Get Borrow Order Info
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-fixed/borrow-order-info

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID
orderCurrency	false	string	Loan coin name
state	false	string	Borrow order status, 1: matching; 2: partially filled and cancelled; 3: Fully filled; 4: Cancelled
term	false	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> annualRate	string	Annual rate for the borrowing
> orderId	long	Loan order ID
> orderTime	string	Order created time
> filledQty	string	Filled qty
> orderQty	string	Order qty
> orderCurrency	string	Coin name
> state	integer	Borrow order status, 1: matching; 2: partially filled and cancelled; 3: Fully filled; 4: Cancelled; 5: fail
> term	integer	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/borrow-order-info?orderId=13010 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752655239825
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "annualRate": "0.01",
                "filledQty": "0",
                "orderCurrency": "MANA",
                "orderId": 13010,
                "orderQty": "2000",
                "orderTime": "1752654035179",
                "state": 1,
                "term": 30
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1752655241090
}

Get Supply Order Info
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-fixed/supply-order-info

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Supply order ID
orderCurrency	false	string	Supply coin name
state	false	string	Supply order status, 1: matching; 2: partially filled and cancelled; 3: Fully filled; 4: Cancelled
term	false	string	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> annualRate	string	Annual rate for the supply
> orderId	long	Supply order ID
> orderTime	string	Order created time
> filledQty	string	Filled qty
> orderQty	string	Order qty
> orderCurrency	string	Coin name
> state	integer	Supply order status, 1: matching; 2: partially filled and cancelled; 3: Fully filled; 4: Cancelled; 5: fail
> term	integer	Fixed term 7: 7 days; 14: 14 days; 30: 30 days; 90: 90 days; 180: 180 days
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/supply-order-info?orderId=13564 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752655992606
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "annualRate": "0.01",
                "filledQty": "800",
                "orderCurrency": "USDT",
                "orderId": 13564,
                "orderQty": "1020",
                "orderTime": "1752482751043",
                "state": 2,
                "term": 7
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1752655993869
}

Repay
Permission: "Spot trade"
UID rate limit: 1 req / second

HTTP Request
POST /v5/crypto-loan-fixed/fully-repay

Request Parameters
Parameter	Required	Type	Comments
loanId	false	string	Loan contract ID. Either loanId or loanCurrency needs to be passed
loanCurrency	false	string	Loan coin. Either loanId or loanCurrency needs to be passed
Response Parameters
Parameter	Type	Comments
repayId	string	Repayment transaction ID
Request Example
HTTP
 
  
POST /v5/crypto-loan-fixed/fully-repay HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: XXXXXX
X-BAPI-TIMESTAMP: 1752656296791
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 50

{
    "loanId": "570",
    "loanCurrency": "ETH"
}

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "repayId": "1771"
    },
    "retExtInfo": {},
    "time": 1752569614549
}

Get Repayment History
Permission: "Spot trade"
UID rate limit: 5 req / second

HTTP Request
GET /v5/crypto-loan-fixed/repayment-history

Request Parameters
Parameter	Required	Type	Comments
repayId	false	string	Repayment order ID
loanCurrency	false	string	Loan coin name
limit	false	string	Limit for data size per page. [1, 100]. Default: 10
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
list	array	Object
> details	array	Object
>> loanCurrency	string	Loan coin name
>> repayAmount	long	Repay amount
>> loanId	string	Loan ID. One repayment may involve multiple loan contracts.
> loanCurrency	string	Loan coin name
> repayAmount	long	Repay amount
> repayId	string	Repay order ID
> repayStatus	integer	Status, 1: success, 2: processing, 3: fail
> repayTime	long	Repay time
> repayType	integer	Repay type, 1: repay by user; 2: repay by liquidation; 3: auto repay; 4: overdue repay; 5: repay by delisting; 6: repay by delay liquidation
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/crypto-loan-fixed/repayment-history?repayId=1780 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: XXXXXXX
X-BAPI-TIMESTAMP: 1752714738425
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "ok",
    "result": {
        "list": [
            {
                "details": [
                    {
                        "loanCurrency": "ETH",
                        "loanId": "568",
                        "repayAmount": "0.1"
                    },
                    {
                        "loanCurrency": "ETH",
                        "loanId": "571",
                        "repayAmount": "1.4"
                    }
                ],
                "loanCurrency": "ETH",
                "repayAmount": "1.5",
                "repayId": "1782",
                "repayStatus": 1,
                "repayTime": 1752717174353,
                "repayType": 1
            }
        ],
        "nextPageCursor": "1674"
    },
    "retExtInfo": {},
    "time": 1752717183557
}

Get Product Info
tip
When queried without an API key, this endpoint returns public product data
If your UID is bound with an OTC loan, then you can get your private product data by calling with your API key
If your UID is not bound with an OTC loan but you passed your API key, this endpoint returns public product data
HTTP Request
GET /v5/ins-loan/product-infos

Request Parameters
Parameter	Required	Type	Comments
productId	false	string	Product ID. If not passed, returns all products
Response Parameters
Parameter	Type	Comments
marginProductInfo	array	Object
> productId	string	Product ID
> leverage	string	The maximum leverage for this loan product
> supportSpot	integer	Whether or not Spot is supported. 0:false; 1:true
> supportContract	integer	Whether USDT Perpetuals are supported. 0:false; 1:true
> supportMarginTrading	integer	Whether or not Spot margin trading is supported. 0:false; 1:true
> deferredLiquidationLine	string	Line for deferred liquidation
> deferredLiquidationTime	string	Time for deferred liquidation
> withdrawLine	string	Restrict line for withdrawal
> transferLine	string	Restrict line for transfer
> spotBuyLine	string	Restrict line for Spot buy
> spotSellLine	string	Restrict line for Spot trading
> contractOpenLine	string	Restrict line for USDT Perpetual open position
> liquidationLine	string	Line for liquidation
> stopLiquidationLine	string	Line for stop liquidation
> contractLeverage	string	The allowed default leverage for USDT Perpetual
> transferRatio	string	The transfer ratio for loan funds to transfer from Spot wallet to Contract wallet
> spotSymbols	array	The whitelist of spot trading pairs
If supportSpot="0", then it returns "[]"
If empty array, then you can trade any symbols
If not empty, then you can only trade listed symbols
> contractSymbols	array	The whitelist of contract trading pairs
If supportContract="0", then it returns "[]"
If empty array, then you can trade any symbols
If not empty, then you can only trade listed symbols
> supportUSDCContract	integer	Whether or not USDC contracts are supported. '0':false; '1':true
> supportUSDCOptions	integer	Whether or not Options are supported. '0':false; '1':true
> USDTPerpetualOpenLine	string	Restrict line to open USDT Perpetual position
> USDCContractOpenLine	string	Restrict line to open USDC Contract position
> USDCOptionsOpenLine	string	Restrict line to open Option position
> USDTPerpetualCloseLine	string	Restrict line to trade USDT Perpetual
> USDCContractCloseLine	string	Restrict line to trade USDC Contract
> USDCOptionsCloseLine	string	Restrict line to trade Option
> USDCContractSymbols	array	The whitelist of USDC contract trading pairs
If supportContract="0", then it returns "[]"
If no whitelist symbols, it is [], and you can trade any
If supportUSDCContract="0", it is []
> USDCOptionsSymbols	array	The whitelist of Option symbols
If supportContract="0", then it returns "[]"
If no whitelisted, it is [], and you can trade any
If supportUSDCOptions="0", it is []
> marginLeverage	string	The allowable maximum leverage for Spot margin trading. If supportMarginTrading=0, then it returns ""
> USDTPerpetualLeverage	array	Object
If supportContract="0", it is []
If no whitelist USDT perp symbols, it returns all trading symbols and leverage by default
If there are whitelist symbols, it return those whitelist data
>> symbol	string	Symbol name
>> leverage	string	Maximum leverage
> USDCContractLeverage	array	Object
If supportUSDCContract="0", it is []
If no whitelist USDC contract symbols, it returns all trading symbols and leverage by default
If there are whitelist symbols, it return those whitelist data
>> symbol	string	Symbol name
>> leverage	string	Maximum leverage
Request Example
HTTP
 
  
GET /v5/ins-loan/product-infos?productId=91 HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "marginProductInfo": [
            {
                "productId": "91",
                "leverage": "4.00000000",
                "supportSpot": 1,
                "supportContract": 0,
                "withdrawLine": "",
                "transferLine": "",
                "spotBuyLine": "",
                "spotSellLine": "",
                "contractOpenLine": "",
                "liquidationLine": "0.75",
                "stopLiquidationLine": "0.35000000",
                "contractLeverage": "0",
                "transferRatio": "0",
                "spotSymbols": [],
                "contractSymbols": [],
                "supportUSDCContract": 0,
                "supportUSDCOptions": 0,
                "USDTPerpetualOpenLine": "",
                "USDCContractOpenLine": "",
                "USDCOptionsOpenLine": "",
                "USDTPerpetualCloseLine": "",
                "USDCContractCloseLine": "",
                "USDCOptionsCloseLine": "",
                "USDCContractSymbols": [],
                "USDCOptionsSymbols": [],
                "marginLeverage": "0",
                "USDTPerpetualLeverage": [],
                "USDCContractLeverage": [],
                "deferredLiquidationLine":"",
                "deferredLiquidationTime":"",
            }
        ]
    },
    "retExtInfo": {},
    "time": 1689747746332
}

Get Margin Coin Info
tip
When queried without an API key, this endpoint returns public margin data
If your UID is bound with an OTC loan, then you can get your private margin data by calling with your API key
If your UID is not bound with an OTC loan but you passed your API key, this endpoint returns public margin data
HTTP Request
GET /v5/ins-loan/ensure-tokens-convert

Request Parameters
Parameter	Required	Type	Comments
productId	false	string	Product ID. If not passed, returns all margin products. For spot, it returns coins with a convertRatio greater than 0.
Response Parameters
Parameter	Type	Comments
marginToken	array	Object
> productId	string	Product Id
> tokenInfo	array	Spot margin coin
>> token	string	Margin coin
>> convertRatioList	array	Margin coin convert ratio List
>>> ladder	string	ladder
>>> convertRatio	string	Margin coin convert ratio
Request Example
HTTP
 
  
GET /v5/ins-loan/ensure-tokens-convert HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "marginToken": [
            {
                "productId": "81",
                "tokenInfo": [
                    {
                        "token": "USDT",
                        "convertRatioList": [
                            {
                                "ladder": "0-500",
                                "convertRatio": "0.95"
                            },
                            {
                                "ladder": "500-1000",
                                "convertRatio": "0.9"
                            },
                            {
                                "ladder": "1000-2000",
                                "convertRatio": "0.8"
                            },
                            {
                                "ladder": "2000-4000",
                                "convertRatio": "0.7"
                            },
                            {
                                "ladder": "4000-99999999999",
                                "convertRatio": "0.6"
                            }
                        ]
                    }
                  ...
                ]
            },
            {
                "productId": "82",
                "tokenInfo": [
                    ...
                    {
                        "token": "USDT",
                        "convertRatioList": [
                            {
                                "ladder": "0-1000",
                                "convertRatio": "0.7"
                            },
                            {
                                "ladder": "1000-2000",
                                "convertRatio": "0.65"
                            },
                            {
                                "ladder": "2000-99999999999",
                                "convertRatio": "0.6"
                            }
                        ]
                    }
                ]
            },
            {
                "productId": "84",
                "tokenInfo": [
                    ...
                    {
                        "token": "BTC",
                        "convertRatioList": [
                            {
                                "ladder": "0-1000",
                                "convertRatio": "1"
                            },
                            {
                                "ladder": "1000-5000",
                                "convertRatio": "0.9"
                            },
                            {
                                "ladder": "5000-99999999999",
                                "convertRatio": "0.55"
                            }
                        ]
                    }
                ]
            }
        ]
    },
    "retExtInfo": {},
    "time": 1683276016497
}

Get Loan Orders
Get up to 2 years worth of historical loan orders.

HTTP Request
GET /v5/ins-loan/loan-order

Request Parameters
Parameter	Required	Type	Comments
orderId	false	string	Loan order ID. If not passed, returns all orders sorted by loanTime in descending order
startTime	false	integer	The start timestamp (ms)
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size. [1, 100], Default: 10
Response Parameters
Parameter	Type	Comments
loanInfo	array	Object
> orderId	string	Loan order ID
> orderProductId	string	Product ID
> parentUid	string	The designated UID that was used to bind with the INS loan
> loanTime	string	Loan timestamp, in milliseconds
> loanCoin	string	Loan coin
> loanAmount	string	Loan amount
> unpaidAmount	string	Unpaid principal
> unpaidInterest	string	Unpaid interest
> repaidAmount	string	Repaid principal
> repaidInterest	string	Repaid interest
> interestRate	string	Daily interest rate
> status	string	1：outstanding; 2：paid off
> leverage	string	The maximum leverage for this loan product
> supportSpot	string	Whether to support spot. 0:false; 1:true
> supportContract	string	Whether to support contract . 0:false; 1:true
> withdrawLine	string	Restrict line for withdrawal
> transferLine	string	Restrict line for transfer
> spotBuyLine	string	Restrict line for SPOT buy
> spotSellLine	string	Restrict line for SPOT sell
> contractOpenLine	string	Restrict line for USDT Perpetual open position
> deferredLiquidationLine	string	Line for deferred liquidation
> deferredLiquidationTime	string	Time for deferred liquidation
> reserveToken	string	Reserve token
> reserveQuantity	string	Reserve token qty
> liquidationLine	string	Line for liquidation
> stopLiquidationLine	string	Line for stop liquidation
> contractLeverage	string	The allowed default leverage for USDT Perpetual
> transferRatio	string	The transfer ratio for loan funds to transfer from Spot wallet to Contract wallet
> spotSymbols	array	The whitelist of spot trading pairs. If there is no whitelist, then "[]"
> contractSymbols	array	The whitelist of contract trading pairs
If supportContract="0", then this is "[]"
If there is no whitelist, this is "[]"
> supportUSDCContract	string	Whether to support USDC contract. "0":false; "1":true
> supportUSDCOptions	string	Whether to support Option. "0":false; "1":true
> supportMarginTrading	string	Whether to support Spot margin trading. "0":false; "1":true
> USDTPerpetualOpenLine	string	Restrict line to open USDT Perpetual position
> USDCContractOpenLine	string	Restrict line to open USDC Contract position
> USDCOptionsOpenLine	string	Restrict line to open Option position
> USDTPerpetualCloseLine	string	Restrict line to trade USDT Perpetual position
> USDCContractCloseLine	string	Restrict line to trade USDC Contract position
> USDCOptionsCloseLine	string	Restrict line to trade Option position
> USDCContractSymbols	array	The whitelist of USDC contract trading pairs
If no whitelist symbols, it is [], and you can trade any
If supportUSDCContract="0", it is []
> USDCOptionsSymbols	array	The whitelist of Option symbols
If no whitelisted, it is [], and you can trade any
If supportUSDCOptions="0", it is []
> marginLeverage	string	The allowable maximum leverage for Spot margin
> USDTPerpetualLeverage	array	Object
If supportContract="0", it is []
If no whitelist USDT perp symbols, it returns all trading symbols and leverage by default
If there are whitelist symbols, it return those whitelist data
>> symbol	string	Symbol name
>> leverage	string	Maximum leverage
> USDCContractLeverage	array	Object
If supportUSDCContract="0", it is []
If no whitelist USDC contract symbols, it returns all trading symbols and leverage by default
If there are whitelist symbols, it return those whitelist data
>> symbol	string	Symbol name
>> leverage	string	Maximum leverage
Request Example
HTTP
 
  
GET /v5/ins-loan/loan-order HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1678687874060
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "loanInfo": [
            {
                "orderId": "1468005106166530304",
                "orderProductId": "96",
                "parentUid": "1631521",
                "loanTime": "1689735916000",
                "loanCoin": "USDT",
                "loanAmount": "204",
                "unpaidAmount": "52.07924201",
                "unpaidInterest": "0",
                "repaidAmount": "151.92075799",
                "repaidInterest": "0",
                "interestRate": "0.00019178",
                "status": "1",
                "leverage": "4",
                "supportSpot": "1",
                "supportContract": "1",
                "withdrawLine": "",
                "transferLine": "",
                "spotBuyLine": "0.71",
                "spotSellLine": "0.71",
                "contractOpenLine": "0.71",
                "liquidationLine": "0.75",
                "stopLiquidationLine": "0.35000000",
                "contractLeverage": "7",
                "transferRatio": "1",
                "spotSymbols": [],
                "contractSymbols": [],
                "supportUSDCContract": "1",
                "supportUSDCOptions": "1",
                "USDTPerpetualOpenLine": "0.71",
                "USDCContractOpenLine": "0.71",
                "USDCOptionsOpenLine": "0.71",
                "USDTPerpetualCloseLine": "0.71",
                "USDCContractCloseLine": "0.71",
                "USDCOptionsCloseLine": "0.71",
                "USDCContractSymbols": [],
                "USDCOptionsSymbols": [],
                "deferredLiquidationLine":"",
                "deferredLiquidationTime":"",
                "marginLeverage": "4",
                "USDTPerpetualLeverage": [
                    {
                        "symbol": "SUSHIUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "INJUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "RDNTUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "ZRXUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "HIGHUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "WAVESUSDT",
                        "leverage": "7"
                    },
                    ...
                    {
                        "symbol": "ACHUSDT",
                        "leverage": "7"
                    },
                    {
                        "symbol": "SUNUSDT",
                        "leverage": "7"
                    }
                ],
                "USDCContractLeverage": [
                    {
                        "symbol": "BTCPERP",
                        "leverage": "8"
                    },
                    {
                        "symbol": "BTC-Futures",
                        "leverage": "8"
                    },
                    ...
                    {
                        "symbol": "ETH-Futures",
                        "leverage": "8"
                    },
                    {
                        "symbol": "SOLPERP",
                        "leverage": "8"
                    },
                    {
                        "symbol": "ETHPERP",
                        "leverage": "8"
                    }
                ]
            }
        ]
    },
    "retExtInfo": {},
    "time": 1689745773187
}

Get Repayment Orders
Get a list of your loan repayment orders (orders which repaid the loan).

tip
Get the past 2 years data by default
Get up to the past 2 years of data
HTTP Request
GET /v5/ins-loan/repaid-history

Request Parameters
Parameter	Required	Type	Comments
startTime	false	integer	The start timestamp (ms)
endTime	false	integer	The end timestamp (ms)
limit	false	integer	Limit for data size. [1, 100]. Default: 100
Response Parameters
Parameter	Type	Comments
repayInfo	array	Object
> repayOrderId	string	Repaid order ID
> repaidTime	string	Repaid timestamp (ms)
> token	string	Repaid coin
> quantity	string	Repaid principle
> interest	string	Repaid interest
> businessType	string	Repaid type. 1：normal repayment; 2：repaid by liquidation
> status	string	1：success; 2：fail
Request Example
HTTP
 
  
GET /v5/ins-loan/repaid-history HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN-TYPE: 2
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1678687944725
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "repayInfo": [
            {
                "repayOrderId": "8189",
                "repaidTime": "1663126393000",
                "token": "USDT",
                "quantity": "30000",
                "interest": "0",
                "businessType": "1",
                "status": "1"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1669366648366
}

Get LTV
Get your loan-to-value (LTV) ratio.

HTTP Request
GET /v5/ins-loan/ltv-convert

Request Parameters
None

Response Parameters
Parameter	Type	Comments
ltvInfo	array	Object
> ltv	string	Risk rate
ltv is calculated in real time
If you have an INS loan, it is highly recommended to query this data every second. Liquidation occurs when it reachs 0.9 (90%)
> rst	string	Remaining liquidation time (UTC time in seconds). When it is not triggered, it is displayed as an empty string.
> parentUid	string	The designated Risk Unit ID that was used to bind with the INS loan
> subAccountUids	array	Bound user ID
> unpaidAmount	string	Total debt(USDT)
> unpaidInfo	array	Debt details
>> token	string	coin
>> unpaidQty	string	Unpaid principle
>> unpaidInterest	string	Useless field, please ignore this for now
> balance	string	Total asset (margin coins converted to USDT). Please read here to understand the calculation
> balanceInfo	array	Asset details
>> token	string	Margin coin
>> price	string	Margin coin price
>> qty	string	Margin coin quantity
>> convertedAmount	string	Margin conversion amount
Request Example
HTTP
 
  
GET /v5/ins-loan/ltv-convert HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1686638165351
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "ltvInfo": [
            {
                "ltv": "0.75",
                "rst": "",
                "parentUid": "xxxxx",
                "subAccountUids": [
                    "60568258"
                ],
                "unpaidAmount": "30",
                "unpaidInfo": [
                    {
                        "token": "USDT",
                        "unpaidQty": "30",
                        "unpaidInterest": "0"
                    }
                ],
                "balance": "40",
                "balanceInfo": [
                    {
                        "token": "USDT",
                        "price": "1",
                        "qty": "40",
                        "convertedAmount": "40"
                    }
                ]
            }
        ]
    },
    "retExtInfo": {},
    "time": 1686638166323
}

Bind Or Unbind UID
For the institutional loan product, you can bind new UIDs to the risk unit or unbind UID from the risk unit.

info
The risk unit designated UID cannot be unbound.
The UID you want to bind must be upgraded to UTA Pro.
HTTP Request
POST /v5/ins-loan/association-uid

Request Parameters
Parameter	Required	Type	Comments
uid	true	string	UID
Bind
a) the key used must be from one of UIDs in the risk unit;
b) input UID must not have an INS loan
Unbind
a) the key used must be from one of UIDs in the risk unit;
b) input UID cannot be the same as the UID used to access the API
operate	true	string	0: bind, 1: unbind
Response Parameters
Parameter	Type	Comments
uid	string	UID
operate	string	0: bind, 1: unbind
Request Example
HTTP
 
  
POST /v5/ins-loan/association-uid HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1699257853101
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
Content-Type: application/json
Content-Length: 43

{
    "uid": "592324",
    "operate": "0"
}

Response Example
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "uid": "592324",
        "operate": "0"
    },
    "retExtInfo": {},
    "time": 1699257746135
}

Get Earning
info
Use exchange broker master account to query
The data can support up to past 1 months until T-1. To extract data from over a month a  , please contact your Relationship Manager
begin & end are either entered at the same time or not entered, and latest 7 days data are returned by default
API rate limit: 10 req / sec

HTTP Request
GET /v5/broker/earnings-info

Request Parameters
Parameter	Required	Type	Comments
bizType	false	string	Business type. SPOT, DERIVATIVES, OPTIONS, CONVERT
begin	false	string	Begin date, in the format of YYYYMMDD, e.g, 20231201, search the data from 1st Dec 2023 00:00:00 UTC (include)
end	false	string	End date, in the format of YYYYMMDD, e.g, 20231201, search the data before 2nd Dec 2023 00:00:00 UTC (exclude)
uid	false	string	
To get results for a specific subaccount: Enter the subaccount UID
To get results for all subaccounts: Leave the field empty
limit	false	integer	Limit for data size per page. [1, 1000]. Default: 1000
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
totalEarningCat	Object	Cate  ry statistics for total earning data
> spot	array	Object. Earning for Spot trading. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
> derivatives	array	Object. Earning for Derivatives trading. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
> options	array	Object. Earning for Option trading. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
> convert	array	Object. Earning for Convert trading. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
> total	array	Object. Sum earnings of all cate  ries. If do not have any rebate, keep empty array
>> coin	string	Rebate coin name
>> earning	string	Rebate amount of the coin
details	array	Object. Detailed trading information for each sub UID and each cate  ry
> userId	string	Sub UID
> bizType	string	Business type. SPOT, DERIVATIVES, OPTIONS, CONVERT
> symbol	string	Symbol name
> coin	string	Rebate coin name
> earning	string	Rebate amount
> markupEarning	string	Earning generated from markup fee rate
> baseFeeEarning	string	Earning generated from base fee rate
> orderId	string	Order ID
> execId	string	Trade ID
> execTime	string	Order execution timestamp (ms)
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/broker/earnings-info?begin=20231129&end=20231129&uid=117894077 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1701399431920
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: 32d2aa1bc205ddfb89849b85e2a8b7e23b1f8f69fe95d6f2cb9c87562f9086a6
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "totalEarningCat": {
            "spot": [],
            "derivatives": [
                {
                    "coin": "USDT",
                    "earning": "0.00027844"
                }
            ],
            "options": [],
            "total": [
                {
                    "coin": "USDT",
                    "earning": "0.00027844"
                }
            ]
        },
        "details": [
            {
                "userId": "117894077",
                "bizType": "DERIVATIVES",
                "symbol": "DOGEUSDT",
                "coin": "USDT",
                "earning": "0.00016166",
                "markupEarning": "0.000032332",
                "baseFeeEarning": "0.000129328",
                "orderId": "ec2132f2-a7e0-4a0c-9219-9f3cbcd8e878",
                "execId": "c8f418a0-2ccc-594f-ae72-effedf24d0c4",
                "execTime": "1701275846033"
            },
            {
                "userId": "117894077",
                "bizType": "DERIVATIVES",
                "symbol": "TRXUSDT",
                "coin": "USDT",
                "earning": "0.00011678",
                "markupEarning": "0.000023356",
                "baseFeeEarning": "0.000093424",
                "orderId": "28b29c2b-ba14-450e-9ce7-3cee0c1fa6da",
                "execId": "632c7705-7f3a-5350-b69c-d41a8b3d0697",
                "execTime": "1701245285017"
            }
        ],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1701398193964
}

Get Account Info
info
Use exchange broker master account to query
API rate limit: 10 req / sec

HTTP Request
GET /v5/broker/account-info

Request Parameters
None

Response Parameters
Parameter	Type	Comments
subAcctQty	string	The qty of sub account has been created
maxSubAcctQty	string	The max limit of sub account can be created
baseFeeRebateRate	Object	Rebate percentage of the base fee
> spot	string	Rebate percentage of the base fee for spot, e.g., 10.00%
> derivatives	string	Rebate percentage of the base fee for derivatives, e.g., 10.00%
markupFeeRebateRate	Object	Rebate percentage of the mark up fee
> spot	string	Rebate percentage of the mark up fee for spot, e.g., 10.00%
> derivatives	string	Rebate percentage of the mark up fee for derivatives, e.g., 10.00%
> convert	string	Rebate percentage of the mark up fee for convert, e.g., 10.00%
ts	string	System timestamp (ms)
Request Example
HTTP
 
  
GET /v5/broker/account-info HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1701399431920
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "subAcctQty": "2",
        "maxSubAcctQty": "20",
        "baseFeeRebateRate": {
            "spot": "10.0%",
            "derivatives": "10.0%"
        },
        "markupFeeRebateRate": {
            "spot": "6.00%",
            "derivatives": "9.00%",
            "convert": "3.00%",
        },
        "ts": "1701395633402"
    },
    "retExtInfo": {},
    "time": 1701395633403
}

Get Sub Account Deposit Records
Exchange broker can query subaccount's deposit records by main UID's API key without specifying uid.

API rate limit: 300 req / min

tip
endTime - startTime should be less than 30 days. Queries for the last 30 days worth of records by default.

HTTP Request
GET /v5/broker/asset/query-sub-member-deposit-record

Request Parameters
Parameter	Required	Type	Comments
id	false	string	Internal ID: Can be used to uniquely identify and filter the deposit. When combined with other parameters, this field takes the highest priority
txID	false	string	Transaction ID: Please note that data generated before Jan 1, 2024 cannot be queried using txID
subMemberId	false	string	Sub UID
coin	false	string	Coin, uppercase only
startTime	false	integer	The start timestamp (ms) Note: the query logic is actually effective based on second level
endTime	false	integer	The end timestamp (ms) Note: the query logic is actually effective based on second level
limit	false	integer	Limit for data size per page. [1, 50]. Default: 50
cursor	false	string	Cursor. Use the nextPageCursor token from the response to retrieve the next page of the result set
Response Parameters
Parameter	Type	Comments
rows	array	Object
> id	string	Unique ID
> subMemberId	string	Sub account user ID
> coin	string	Coin
> chain	string	Chain
> amount	string	Amount
> txID	string	Transaction ID
> status	integer	Deposit status
> toAddress	string	Deposit target address
> tag	string	Tag of deposit target address
> depositFee	string	Deposit fee
> successAt	string	Last updated time
> confirmations	string	Number of confirmation blocks
> txIndex	string	Transaction sequence number
> blockHash	string	Hash number on the chain
> batchReleaseLimit	string	The deposit limit for this coin in this chain. "-1" means no limit
> depositType	string	The deposit type. 0: normal deposit, 10: the deposit reaches daily deposit limit, 20: abnormal deposit
> fromAddress	string	From address of deposit, only shown when the deposit comes from on-chain and from address is unique, otherwise gives ""
> taxDepositRecordsId	string	This field is used for tax purposes by Bybit EU (Austria) users， declare tax id
> taxStatus	integer	This field is used for tax purposes by Bybit EU (Austria) users
0: No reporting required
1: Reporting pending
2: Reporting completed
nextPageCursor	string	Refer to the cursor request parameter
Request Example
HTTP
 
  
GET /v5/broker/asset/query-sub-member-deposit-record?coin=USDT&limit=1 HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672192441294
X-BAPI-RECV-WINDOW: 5000

Response Example
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "rows": [],
        "nextPageCursor": ""
    },
    "retExtInfo": {},
    "time": 1672192441742
}

Query Voucher Spec
HTTP Request
POST /v5/broker/award/info

Request Parameters
Parameter	Required	Type	Comments
id	true	string	Voucher ID
Response Parameters
Parameter	Type	Comments
id	string	Voucher ID
coin	string	Coin
amountUnit	string	
AWARD_AMOUNT_UNIT_USD
AWARD_AMOUNT_UNIT_COIN
productLine	string	Product line
subProductLine	string	Sub product line
totalAmount	Object	Total amount of voucher
usedAmount	string	Used amount of voucher
Request Example
HTTP
 
  
POST /v5/broker/award/info HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1726107086048
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 22

{
    "id": "80209"
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "id": "80209",
        "coin": "USDT",
        "amountUnit": "AWARD_AMOUNT_UNIT_USD",
        "productLine": "PRODUCT_LINE_CONTRACT",
        "subProductLine": "SUB_PRODUCT_LINE_CONTRACT_DEFAULT",
        "totalAmount": "10000",
        "usedAmount": "100"
    },
    "retExtInfo": {},
    "time": 1726107086313
}

Issue Voucher
HTTP Request
POST /v5/broker/award/distribute-award

Request Parameters
Parameter	Required	Type	Comments
accountId	true	string	User ID
awardId	true	string	Voucher ID
specCode	true	string	Customised unique spec code, up to 8 characters
amount	true	string	Issue amount
Spot airdrop supports up to 16 decimals
Other types supports up to 4 decimals
brokerId	true	string	Broker ID
Response Parameters
None

Request Example
HTTP
 
  
POST /v5/broker/award/distribute-award HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1726110531734
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 128

{
    "accountId": "2846381",
    "awardId": "123456",
    "specCode": "award-001",
    "amount": "100",
    "brokerId": "v-28478"
}

Response Example
{
    "retCode": 0,
    "retMsg": ""
}

Query Issued Voucher
HTTP Request
POST /v5/broker/award/distribution-record

Request Parameters
Parameter	Required	Type	Comments
accountId	true	string	User ID
awardId	true	string	Voucher ID
specCode	true	string	Customised unique spec code, up to 8 characters
withUsedAmount	false	boolean	Whether to return the amount used by the user
true
false (default)
Response Parameters
Parameter	Type	Comments
accountId	string	User ID
awardId	string	Voucher ID
specCode	string	Spec code
amount	string	Amount of voucher
isClaimed	boolean	true, false
startAt	string	Claim start timestamp (sec)
endAt	string	Claim end timestamp (sec)
effectiveAt	string	Voucher effective timestamp (sec) after claimed
ineffectiveAt	string	Voucher inactive timestamp (sec) after claimed
usedAmount	string	Amount used by the user
Request Example
HTTP
 
  
POST /v5/broker/award/distribution-record HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1726112099846
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 111

{
    "accountId": "5714139",
    "awardId": "189528",
    "specCode": "demo000",
    "withUsedAmount": false
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "accountId": "5714139",
        "awardId": "189528",
        "specCode": "demo000",
        "amount": "1",
        "isClaimed": true,
        "startAt": "1725926400",
        "endAt": "1733788800",
        "effectiveAt": "1726531200",
        "ineffectiveAt": "1733817600",
        "usedAmount": "",
    }
}

Get Product Info
info
Does not need authentication.

Bybit Saving FAQ

HTTP Request
GET /v5/earn/product

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	FlexibleSaving,OnChain
Remarks: currently, only flexible savings and on chain is supported
coin	false	string	Coin name, uppercase only
Response Parameters
Parameter	Type	Comments
list	array	Object
> cate  ry	string	FlexibleSaving,OnChain
> estimateApr	string	Estimated APR, e.g., 3%, 4.25%
Remarks: 1)The Est. APR provides a dynamic preview of your potential returns, updated every 10 minutes in response to market conditions.
2) Please note that this is an estimate and may differ from the actual APR you will receive.
3) Platform Reward APRs are not shown
> coin	string	Coin name
> minStakeAmount	string	Minimum stake amount
> maxStakeAmount	string	Maximum stake amount
> precision	string	Amount precision
> productId	string	Product ID
> status	string	Available, NotAvailable
> bonusEvents	Array	Bonus
>> apr	string	Yesterday's Rewards APR
>> coin	string	Reward coin
>> announcement	string	Announcement link
> minRedeemAmount	string	Minimum redemption amount. Only has value in Onchain LST mode
> maxRedeemAmount	string	Maximum redemption amount. Only has value in Onchain LST mode
> duration	string	Fixed,Flexible. Product Type
> term	int	Unit: Day. Only when duration = Fixed for OnChain
> swapCoin	string	swap coin. Only has value in Onchain LST mode
> swapCoinPrecision	string	swap coin precision. Only has value in Onchain LST mode
> stakeExchangeRate	string	Estimated stake exchange rate. Only has value in Onchain LST mode
> redeemExchangeRate	string	Estimated redeem exchange rate. Only has value in Onchain LST mode
> rewardDistributionType	string	Simple: Simple interest, Compound: Compound interest, Other: LST. Only has value for Onchain
> rewardIntervalMinute	int	Frequency of reward distribution (minutes)
> redeemProcessingMinute	string	Estimated redemption minutes
> stakeTime	string	Staking on-chain time, in milliseconds
> interestCalculationTime	string	Interest accrual time, in milliseconds
Request Example
HTTP
 
  
GET /v5/earn/product?cate  ry=FlexibleSaving&coin=BTC HTTP/1.1
Host: api-testnet.bybit.com

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "list": [
            {
                "cate  ry": "FlexibleSaving",
                "estimateApr": "3%",
                "coin": "BTC",
                "minStakeAmount": "0.001",
                "maxStakeAmount": "10",
                "precision": "8",
                "productId": "430",
                "status": "Available",
                "bonusEvents": [],
                "minRedeemAmount": "",
                "maxRedeemAmount": "",
                "duration": "",
                "term": 0,
                "swapCoin": "",
                "swapCoinPrecision": "",
                "stakeExchangeRate": "",
                "redeemExchangeRate": "",
                "rewardDistributionType": "",
                "rewardIntervalMinute": 0,
                "redeemProcessingMinute": 0,
                "stakeTime": "",
                "interestCalculationTime": ""
            }
        ]
    },
    "retExtInfo": {},
    "time": 1739935669110
}

Stake / Redeem
info
API key needs "Earn" permission

note
In times of high demand for loans in the market for a specific cryptocurrency, the redemption of the principal may encounter delays and is expected to be processed within 48 hours. The redemption of on-chain products may take up to a few days to complete. Once the redemption request is initiated, it cannot be cancelled.

HTTP Request
POST /v5/earn/place-order

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	FlexibleSaving,OnChain
Remarks: currently, only flexible savings and on chain is supported
orderType	true	string	Stake, Redeem
accountType	true	string	FUND, UNIFIED. Onchain only supports FUND
amount	true	string	
Stake amount needs to satisfy minStake and maxStake
Both stake and redeem amount need to satify precision requirement
coin	true	string	Coin name
productId	true	string	Product ID
orderLinkId	true	string	Customised order ID, used to prevent from replay
support up to 36 characters
The same orderLinkId can't be used in 30 mins
redeemPositionId	false	string	The position ID that the user needs to redeem. Only is required in Onchain non-LST mode
Response Parameters
Parameter	Type	Comments
orderId	string	Order ID
orderLinkId	string	Order link ID
Request Example
HTTP
 
  
POST /v5/earn/place-order HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1739936605822
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 190

{
    "cate  ry": "FlexibleSaving",
    "orderType": "Redeem",
    "accountType": "FUND",
    "amount": "0.35",
    "coin": "BTC",
    "productId": "430",
    "orderLinkId": "btc-earn-001"
}

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "orderId": "0572b030-6a0b-423f-88c4-b6ce31c0c82d",
        "orderLinkId": "btc-earn-001"
    },
    "retExtInfo": {},
    "time": 1739936607117
}

Get Stake/Redeem Order History
info
API key needs "Earn" permission

HTTP Request
GET /v5/earn/order

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	FlexibleSaving,OnChain
Remarks: currently, only flexible savings and OnChain is supported
orderId	false	string	Order ID
either orderId or orderLinkId is required
if both are passed, make sure they're matched, otherwise returning empty result
orderLinkId	false	string	Order link ID
Remarks: Always return the latest one if order link id is ever reused when querying by orderLinkId only
Response Parameters
Parameter	Type	Comments
list	array	Object
> coin	string	Coin name
> orderValue	string	amount
> orderType	string	Redeem, Stake
> orderId	string	Order ID
> orderLinkId	string	Order link ID
> status	string	Order status Success, Fail, Pending
> createdAt	string	Order created time, in milliseconds
> productId	string	Product ID
> updatedAt	string	Order updated time, in milliseconds
> swapOrderValue	string	Swap order value. Only for LST Onchain.
> estimateRedeemTime	string	Estimate redeem time, in milliseconds. Only for Onchain
> estimateStakeTime	string	Estimate stake time, in milliseconds. Only for Onchain
Request Example
HTTP
 
  
GET /v5/earn/order?orderId=9640dc23-df1a-448a-ad24-e1a48028a51f&cate  ry=OnChain HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1739937044221
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "list": [
            {
                "coin": "BTC",
                "orderValue": "1",
                "orderType": "Stake",
                "orderId": "9640dc23-df1a-448a-ad24-e1a48028a51f",
                "orderLinkId": "cjm2",
                "status": "Success",
                "createdAt": "1744166831000",
                "productId": "8",
                "updatedAt": "1744166832000",
                "swapOrderValue": "",
                "estimateRedeemTime": "",
                "estimateStakeTime": ""
            }
        ]
    },
    "retExtInfo": {},
    "time": 1739937045520
}

Get Staked Position
info
API key needs "Earn" permission

note
For Flexible Saving, fully redeemed position is also returned in the response For Onchain, only active position will be returned in the response

HTTP Request
GET /v5/earn/position

Request Parameters
Parameter	Required	Type	Comments
cate  ry	true	string	FlexibleSaving,OnChain
productId	false	string	Product ID
coin	false	string	Coin name
Response Parameters
Parameter	Type	Comments
list	array	Object
> coin	string	Coin name
> productId	string	Product ID
> amount	string	Total staked amount
> totalPnl	string	Return the profit of the current position. Only has value in Onchain non-LST mode
> claimableYield	string	Yield accrues on an hourly basis and is distributed at 00:30 UTC daily. If you unstake your assets before yield distribution, any undistributed yield will be credited to your account along with your principal. Onchain products do not return values
> id	string	Position Id. Only for Onchain
> status	string	Processing,Active. Only for Onchain
> orderId	string	Order Id. Only for Onchain
> estimateRedeemTime	string	Estimate redeem time, in milliseconds. Only for Onchain
> estimateStakeTime	string	Estimate stake time, in milliseconds. Only for Onchain
> estimateInterestCalculationTime	string	Estimated Interest accrual time, in milliseconds. Only for Onchain
> settlementTime	string	Settlement time, in milliseconds. Only has value for Onchain Fixed product
Request Example
HTTP
 
  
GET /v5/earn/position?cate  ry=FlexibleSaving&coin=USDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1739944576277
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

Response Example
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "list": [
            {
                "coin": "BTC",
                "productId": "8",
                "amount": "0.1",
                "totalPnl": "0.000027397260273973",
                "claimableYield": "0",
                "id": "326",
                "status": "Active",
                "orderId": "1a5a8945-e042-4dd5-a93f-c0f0577377ad",
                "estimateRedeemTime": "",
                "estimateStakeTime": "",
                "estimateInterestCalculationTime": "1744243200000",
                "settlementTime": "1744675200000"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1739944577575
}

Rate Limit
IP Limit
HTTP IP limit
You are allowed to send 600 requests within a 5-second window per IP by default. This limit applies to all traffic directed to api.bybit.com, api.bybick.com, and local site hostnames such as api.bybit.kz. If you encounter the error "403, access too frequent", it indicates that your IP has exceeded the allowed request frequency. In this case, you should terminate all HTTP sessions and wait for at least 10 minutes. The ban will be lifted automatically.

We do not recommend running your application at the very edge of these limits in case abnormal network activity results in an unexpected violation.

Websocket IP limit
Do not establish more than 500 connections within a 5-minute window. This limit applies to all connections directed to stream.bybit.com as well as local site hostnames such as stream.bybit.kz.
Do not frequently connect and disconnect the connection
Do not establish more than 1,000 connections per IP for market data. The connection limits are counted separately for Spot, Linear, Inverse, and Options markets
API Rate Limit
caution
If you receive "retCode": 10006, "retMsg": "Too many visits!" in the JSON response, you have hit the API rate limit.

The API rate limit is based on the rolling time window per second and UID. In other words, it is per second per UID. Every request to the API returns response header shown in the code panel:

X-Bapi-Limit-Status - your remaining requests for current endpoint
X-Bapi-Limit - your current limit for current endpoint
X-Bapi-Limit-Reset-Timestamp - the timestamp indicating when your request limit resets if you have exceeded your rate_limit. Otherwise, this is just the current timestamp (it may not exactly match timeNow).
Http Response Header Example

▶Response Headers
Content-Type: application/json; charset=utf-8
Content-Length: 141
X-Bapi-Limit: 100
X-Bapi-Limit-Status: 99
X-Bapi-Limit-Reset-Timestamp: 1672738134824

API Rate Limit Table
Trade
Classic account
UTA1.0 Pro
UTA2.0 Pro
Method	Path	Classic account	upgradable
inverse	linear	spot
POST	/v5/order/create	10/s	20/s	Y
/v5/order/amend	10/s	10/s	Y
/v5/order/cancel	10/s	20/s	Y
/v5/order/cancel-all	10/s	20/s	Y
GET	/v5/order/realtime	10/s	20/s	N
/v5/order/history	10/s	20/s	N
/v5/execution/list	10/s	20/s	N
Position
Classic account
UTA1.0 Pro
UTA2.0 Pro
Method	Path	Classic account	upgradable
inverse	linear	spot
GET	/v5/position/list	10/s	-	N
/v5/position/closed-pnl	10/s	-	N
POST	/v5/position/set-leverage	10/s	-	N
Account
Classic account
UTA1.0 Pro
UTA2.0 Pro
Method	Path		Limit	upgradable
GET	/v5/account/contract-transaction-log		10/s	N
/v5/account/wallet-balance	accountType=SPOT	20/s	N
accountType=CONTRACT	10/s	N
/v5/account/fee-rate	cate  ry=linear	10/s	N
cate  ry=spot	5/s	N
cate  ry=option	5/s	N
Asset
Method	Path	Limit	upgradable
GET	/v5/asset/transfer/query-asset-info	60 req/min	N
/v5/asset/transfer/query-transfer-coin-list	60 req/min	N
/v5/asset/transfer/query-inter-transfer-list	60 req/min	N
/v5/asset/transfer/query-sub-member-list	60 req/min	N
/v5/asset/transfer/query-universal-transfer-list	5 req/s	N
/v5/asset/transfer/query-account-coins-balance	5 req/s	N
/v5/asset/deposit/query-record	100 req/min	N
/v5/asset/deposit/query-sub-member-record	300 req/min	N
/v5/asset/deposit/query-address	300 req/min	N
/v5/asset/deposit/query-sub-member-address	300 req/min	N
/v5/asset/withdraw/query-record	300 req/min	N
/v5/asset/coin/query-info	5 req/s	N
/v5/asset/exchange/order-record	600 req/min	N
POST	/v5/asset/transfer/inter-transfer	60 req/min	N
/v5/asset/transfer/save-transfer-sub-member	20 req/s	N
/v5/asset/transfer/universal-transfer	5 req/s	N
/v5/asset/withdraw/create	5 req/s	N
/v5/asset/withdraw/cancel	60 req/min	N
User
Method	Path	Limit	upgradable
POST	v5/user/create-sub-member	1 req/s	N
/v5/user/create-sub-api	1 req/s	N
/v5/user/frozen-sub-member	5 req/s	N
/v5/user/update-api	5 req/s	N
/v5/user/update-sub-api	5 req/s	N
/v5/user/delete-api	5 req/s	N
/v5/user/delete-sub-api	5 req/s	N
GET	/v5/user/query-sub-members	10 req/s	N
/v5/user/query-api	10 req/s	N
/v5/user/aff-customer-info	10 req/s	N
Spot Leverage Token
Method	Path	Limit	Upgradable
GET	/v5/spot-lever-token/order-record	50 req/s	N
POST	/v5/spot-lever-token/purchase	20 req/s	N
POST	/v5/spot-lever-token/redeem	20 req/s	N
Spot Margin Trade (UTA)
For now, there is no limit for endpoints under this cate  ry
Spread Trading
Method	Path	Limit	Upgradable
POST	Create Spread Order	20 req/s	N
POST	Amend Spread Order	20 req/s	N
POST	Cancel Spread Order	20 req/s	N
POST	Cancel All Spread Orders	5 req/s	N
GET	Get Spread Open Orders	50 req/s	N
GET	Get Spread Order History	50 req/s	N
GET	Get Spread Trade History	50 req/s	N
API Rate Limit Rules For VIPs
instructions for batch endpoints
The batch order endpoint, which includes operations for creating, amending, and canceling, has its own rate limit and does not share it with single requests, e.g., let's say the rate limit of single create order endpoint is 100/s, and batch create order endpoint is 100/s, so in this case, I can place 200 linear orders in one second if I use both endpoints to place orders

When cate  ry = linear spot or inverse
API for batch create/amend/cancel order, the frequency of the API will be consistent with the current configuration, but the counting consumption will be consumed according to the actual number of orders. (Number of consumption = number of requests * number of orders included in a single request), and the configuration of business lines is independent of each other.

The batch APIs allows 1-10 orders/request. For example, if a batch order request is made once and contains 5 orders, then the request limit will consume 5.

If part of the last batch of orders requested within 1s exceeds the limit, the part that exceeds the limit will fail, and the part that does not exceed the limit will succeed. For example, in the 1 second, the remaining limit is 5, but a batch request containing 8 orders is placed at this time, then the first 5 orders will be successfully placed, and the 6-8th orders will report an error exceeding the limit, and these orders will fail.

Unified Account
Level\Product	Futures	Option	Spot
Default	10/s	10/s	20/s
VIP 1	20/s	20/s	25/s
VIP 2	40/s	40/s	30/s
VIP 3	60/s	60/s	40/s
VIP 4	60/s	60/s	40/s
VIP 5	60/s	60/s	40/s
VIP Supreme	60/s	60/s	40/s
API Rate Limit Rules For PROs
UPCOMING CHANGES FOR PRO ACCOUNT
Starting August 13, 2025, Bybit will roll out a new institutional API rate limit framework designed to enhance performance for high-frequency trading clients. The new system introduces a centralized institution-level rate cap with flexible per-UID configurations, enabling greater efficiency and scalability. Refer to announcement

UID level:
Unified Account
Level\Product	Futures	Option	Spot
PRO1	200/s	200/s	200/s
PRO2	400/s	400/s	400/s
PRO3	600/s	600/s	600/s
PRO4	800/s	800/s	800/s
PRO5	1000/s	1000/s	1000/s
PRO6	1200/s	1200/s	1200/s
Master and subaccounts level:
Unified Account
Level\Product	Futures	Option	Spot
PRO1	10000/s	10000/s	10000/s
PRO2	20000/s	20000/s	20000/s
PRO3	30000/s	30000/s	30000/s
PRO4	40000/s	40000/s	40000/s
PRO5	50000/s	50000/s	50000/s
PRO6	60000/s	60000/s	60000/s
Set api rate limit
API rate limit: 50 req per second

info
If UID requesting this endpoint is a master account, uids in the input parameter must be subaccounts of the master account.
If UID requesting this endpoint is not a master account, uids in the input parameter must be the UID requesting this endpoint
UID requesting this endpoint must be an institutional user.
up to 10 uids can be submitted in each request
HTTP Request
POST /v5/apilimit/set

Request Parameters
Parameter	Required	Type	Comments
list	true	array	Object
> uids	true	string	Multiple UIDs, separated by commas
> bizType	true	string	Business type
> limit	true	integer	api rate limit per second
Response Parameters
Parameter	Type	Comments
list	array	Object
> uids	string	Multiple UIDs, separated by commas
> bizType	string	Business type
> limit	integer	api rate limit per second
> success	boolean	success or not
> msg	string	result message
Request Example
POST /v5/apilimit/set HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1711420489915
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json

{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "result": [
            {
                "uids": "290118",
                "bizType": "SPOT",
                "limit": 600,
                "success": true,
                "msg": "API limit updated successfully"
            }
        ]
    },
    "retExtInfo": {},
    "time": 1754894296913
}

Query api rate limit
API rate limit: 50 req per second

info
A master account can query api rate limit of its own and subaccounts.
A subaccount can only query its own api rate limit.
HTTP Request
GET /v5/apilimit/query

Request Parameters
Parameter	Required	Type	Comments
uids	true	string	Multiple UIDs, separated by commas
Response Parameters
Parameter	Type	Comments
list	array	Object
> uids	string	Multiple UIDs, separated by commas
> bizType	string	Business type
> limit	integer	api rate limit per second
Request Example
GET /v5/apilimit/query?uids=290118 HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728460942776
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 2

{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "list": [
            {
                "uids": "290118",
                "bizType": "SPOT",
                "limit": 600
            },
            {
                "uids": "290118",
                "bizType": "DERIVATIVES",
                "limit": 400
            }
        ]
    },
    "retExtInfo": {},
    "time": 1754894341984
}

Enums Definitions
locale
de-DE
en-US
es-AR
es-ES
es-MX
fr-FR
kk-KZ
id-ID
uk-UA
ja-JP
ru-RU
th-TH
pt-BR
tr-TR
vi-VN
zh-TW
ar-SA
hi-IN
fil-PH
announcementType
new_crypto
latest_bybit_news
delistings
latest_activities
product_updates
maintenance_updates
new_fiat_listings
other
announcementTag
Spot
Derivatives
Spot Listings
BTC
ETH
Trading Bots
USDC
Leveraged Tokens
USDT
Margin Trading
Partnerships
Launchpad
Upgrades
ByVotes
Delistings
VIP
Futures
Institutions
Options
WEB3
Copy Trading
Earn
Bybit Savings
Dual Asset
Liquidity Mining
Shark Fin
Launchpool
NFT GrabPic
Buy Crypto
P2P Trading
Fiat Deposit
Crypto Deposit
Спот
Спот лістинги
Торгові боти
Токени з кредитним плечем
Маржинальна торгівля
Партнерство
Оновлення
Делістинги
Ф'ючерси
Опціони
Копітрейдинг
Bybit Накопичення
Бівалютні інвестиції
Майнінг ліквідності
Купівля криптовалюти
P2P торгівля
Фіатні депозити
Криптодепозити
Копитрейдинг
Торговые боты
Деривативы
P2P
Спот листинги
Деривативи
MT4
Lucky Draw
Unified Trading Account
Єдиний торговий акаунт
Единый торговый аккаунт
Институциональный трейдинг
Інституціональний трейдинг
Делистинг
cate  ry
Unified Account

spot
linear USDT perpetual, USDT Futures and USDC contract, including USDC perp, USDC futures
inverse Inverse contract, including Inverse perp, Inverse futures
option
Classic Account

linear USDT perp
inverse Inverse contract, including Inverse perp, Inverse futures
spot
orderStatus
open status

New order has been placed successfully
PartiallyFilled
Untriggered Conditional orders are created
closed status

Rejected
PartiallyFilledCanceled Only spot has this order status
Filled
Cancelled In derivatives, orders with this status may have an executed qty
Triggered instantaneous state for conditional orders from Untriggered to New
Deactivated UTA: Spot tp/sl order, conditional order, OCO order are cancelled before they are triggered
timeInForce
GTC   odTillCancel
IOC ImmediateOrCancel
FOK FillOrKill
PostOnly
RPI features:
Exclusive Matching: Only match non-al  rithmic users; no execution against orders from Open API.
Post-Only Mechanism: Act as maker orders, adding liquidity
Lower Priority: Execute after non-RPI orders at the same price level.
Limited Access: Initially for select market makers across multiple pairs.
Order Book Updates: Excluded from API but displayed on the GUI.
createType
CreateByUser
CreateByFutureSpread Spread order
CreateByAdminClosing
CreateBySettle USDC Futures delivery; position closed as a result of the delisting of a contract. This is recorded as a trade but not an order.
CreateByStopOrder Futures conditional order
CreateByTakeProfit Futures take profit order
CreateByPartialTakeProfit Futures partial take profit order
CreateByStopLoss Futures stop loss order
CreateByPartialStopLoss Futures partial stop loss order
CreateByTrailingStop Futures trailing stop order
CreateByLiq Laddered liquidation to reduce the required maintenance margin
CreateByTakeOver_PassThroughIf the position is still subject to liquidation (i.e., does not meet the required maintenance margin level), the position shall be taken over by the liquidation engine and closed at the bankruptcy price.
CreateByAdl_PassThrough Auto-Deleveraging(ADL)
CreateByBlock_PassThrough Order placed via Paradigm
CreateByBlockTradeMovePosition_PassThrough Order created by move position
CreateByClosing The close order placed via web or app position area - web/app
CreateByFGridBot Order created via grid bot - web/app
CloseByFGridBot Order closed via grid bot - web/app
CreateByTWAP Order created by TWAP - web/app
CreateByTVSignal Order created by TV webhook - web/app
CreateByMmRateClose Order created by Mm rate close function - web/app
CreateByMartingaleBot Order created by Martingale bot - web/app
CloseByMartingaleBot Order closed by Martingale bot - web/app
CreateByIceBerg Order created by Ice berg strategy - web/app
CreateByArbitrage Order created by arbitrage - web/app
CreateByDdh Option dynamic delta hedge order - web/app
execType
Trade
AdlTrade Auto-Deleveraging
Funding Funding fee
BustTrade Takeover liquidation
Delivery USDC futures delivery; Position closed by contract delisted
Settle Inverse futures settlement; Position closed due to delisting
BlockTrade
MovePosition
FutureSpread Spread leg execution
UNKNOWN May be returned by a classic account. Cannot query by this type
orderType
Market
Limit
UNKNOWN is not a valid request parameter value. Is only used in some responses. Mainly, it is used when execType is Funding.
stopOrderType
TakeProfit
StopLoss
TrailingStop
Stop
PartialTakeProfit
PartialStopLoss
tpslOrder spot TP/SL order
OcoOrder spot Oco order
MmRateClose On web or app can set MMR to close position
BidirectionalTpslOrder Spot bidirectional tpsl order
tickDirection
PlusTick price rise
ZeroPlusTick trade occurs at the same price as the previous trade, which occurred at a price higher than that for the trade preceding it
MinusTick price drop
ZeroMinusTick trade occurs at the same price as the previous trade, which occurred at a price lower than that for the trade preceding it
interval
1 3 5 15 30 60 120 240 360 720 minute
D day
W week
M month
intervalTime
5min 15min 30min minute
1h 4h hour
1d day
positionIdx
0 one-way mode position
1 Buy side of hedge-mode position
2 Sell side of hedge-mode position
positionStatus
Normal
Liq in the liquidation progress
Adl in the auto-deleverage progress
rejectReason
EC_NoError
EC_Others
EC_UnknownMessageType
EC_MissingClOrdID
EC_Missin  rigClOrdID
EC_ClOrdIDOrigClOrdIDAreTheSame
EC_DuplicatedClOrdID
EC_OrigClOrdIDDoesNotExist
EC_TooLateToCancel
EC_UnknownOrderType
EC_UnknownSide
EC_UnknownTimeInForce
EC_WronglyRouted
EC_MarketOrderPriceIsNotZero
EC_LimitOrderInvalidPrice
EC_NoEnoughQtyToFill
EC_NoImmediateQtyToFill
EC_PerCancelRequest
EC_MarketOrderCannotBePostOnly
EC_PostOnlyWillTakeLiquidity
EC_CancelReplaceOrder
EC_InvalidSymbolStatus
EC_CancelForNoFullFill
EC_BySelfMatch
EC_InCallAuctionStatus used for pre-market order operation, e.g., during 2nd phase of call auction, cancel order is not allowed, when the cancel request is failed to be rejected by trading server, the request will be rejected by matching box finally
EC_QtyCannotBeZero
EC_MarketOrderNoSupportTIF
EC_ReachMaxTradeNum
EC_InvalidPriceScale
EC_BitIndexInvalid
EC_StopBySelfMatch
EC_InvalidSmpType
EC_CancelByMMP
EC_InvalidUserType
EC_InvalidMirrorOid
EC_InvalidMirrorUid
EC_EcInvalidQty
EC_InvalidAmount
EC_LoadOrderCancel
EC_MarketQuoteNoSuppSell
EC_DisorderOrderID
EC_InvalidBaseValue
EC_LoadOrderCanMatch
EC_SecurityStatusFail
EC_ReachRiskPriceLimit
EC_OrderNotExist
EC_CancelByOrderValueZero
EC_CancelByMatchValueZero
EC_ReachMarketPriceLimit
accountType
UTA2.0
UNIFIED Unified Trading Account
FUND Funding Account
UTA1.0
CONTRACT Inverse Derivatives Account (no UDST in this wallet))
UNIFIED Unified Trading Account
FUND Funding Account
Classic account
Also known as the "standard account".

SPOT Spot Account
CONTRACT Derivatives Account (contain USDT in this wallet)
FUND Funding Account
transferStatus
SUCCESS
PENDING
FAILED
depositStatus
0 unknown
1 toBeConfirmed
2 processing
3 success (finalised status of a success deposit)
4 deposit failed
10011 pending to be credited to funding pool
10012 Credited to funding pool successfully
withdrawStatus
SecurityCheck
Pending
success
CancelByUser
Reject
Fail
BlockchainConfirmed
MoreInformationRequired
Unknown a rare status
triggerBy
LastPrice
IndexPrice
MarkPrice
cancelType
CancelByUser
CancelByReduceOnly cancelled by reduceOnly
CancelByPrepareLiq CancelAllBeforeLiq cancelled in order to attempt liquidation prevention by freeing up margin
CancelByPrepareAdl CancelAllBeforeAdl cancelled due to ADL
CancelByAdmin
CancelBySettle cancelled due to delisting contract
CancelByTpSlTsClear TP/SL order cancelled when the position is cleared
CancelBySmp cancelled by SMP
CancelByDCP cancelled by DCP triggering
CancelByRebalance Spread trading: the order price of a single leg order is outside the limit price range.
Options:

CancelByUser
CancelByReduceOnly
CancelAllBeforeLiq cancelled due to liquidation
CancelAllBeforeAdl cancelled due to ADL
CancelBySettle
CancelByCannotAffordOrderCost
CancelByPmTrialMmOverEquity
CancelByAccountBlocking
CancelByDelivery
CancelByMmpTriggered
CancelByCrossSelfMuch
CancelByCrossReachMaxTradeNum
CancelByDCP
CancelBySmp
optionPeriod
BTC: 7,14,21,30,60,90,180,270days
ETH: 7,14,21,30,60,90,180,270days
SOL: 7,14,21,30,60,90days
dataRecordingPeriod
5min 15min 30min minute
1h 4h hour
4d day
contractType
InversePerpetual
LinearPerpetual
LinearFutures USDT/USDC Futures
InverseFutures
status
PreLaunch
Trading
Delivering
Closed
curAuctionPhase
NotStarted Pre-market trading is not started
Finished Pre-market trading is finished
After the auction, if the pre-market contract fails to enter continues trading phase, it will be delisted and phase="Finished"
After the continuous trading, if the pre-market contract fails to be converted to official contract, it will be delisted and phase="Finished"
CallAuction Auction phase of pre-market trading
only timeInForce=GTC, orderType=Limit order is allowed to submit
TP/SL are not supported; Conditional orders are not supported
cannot modify the order at this stage
order price range: [preOpenPrice x 0.5, maxPrice]
CallAuctionNoCancel Auction no cancel phase of pre-market trading
only timeInForce=GTC, orderType=Limit order is allowed to submit
TP/SL are not supported; Conditional orders are not supported
cannot modify and cancel the order at this stage
order price range: Buy [lastPrice x 0.5, markPrice x 1.1], Sell [markPrice x 0.9, maxPrice]
CrossMatching cross matching phase
cannot create, modify and cancel the order at this stage
Candle data is released from this stage
ContinuousTrading Continuous trading phase
There is no restriction to create, amend, cancel orders
orderbook, public trade data is released from this stage
marginTrading
none Regardless of normal account or UTA account, this trading pair does not support margin trading
both For both normal account and UTA account, this trading pair supports margin trading
utaOnly Only for UTA account,this trading pair supports margin trading
normalSpotOnly Only for normal account, this trading pair supports margin trading
copyTrading
none Regardless of normal account or UTA account, this trading pair does not support copy trading
both For both normal account and UTA account, this trading pair supports copy trading
utaOnly Only for UTA account,this trading pair supports copy trading
normalOnly Only for normal account, this trading pair supports copy trading
type(uta-translog)
TRANSFER_IN Assets that transferred into Unified wallet
TRANSFER_OUT Assets that transferred out from Unified wallet
TRADE
SETTLEMENT USDT Perp funding settlement, and USDC Perp funding settlement + USDC 8-hour session settlement
DELIVERY USDC Futures, Option delivery
LIQUIDATION
ADL Auto-Deleveraging
AIRDROP
BONUS Bonus claimed
BONUS_RECOLLECT Bonus expired
FEE_REFUND Trading fee refunded
INTEREST Interest occurred due to borrowing
CURRENCY_BUY Currency convert, and the liquidation for borrowing asset(UTA loan)
CURRENCY_SELL Currency convert, and the liquidation for borrowing asset(UTA loan)
BORROWED_AMOUNT_INS_LOAN
PRINCIPLE_REPAYMENT_INS_LOAN
INTEREST_REPAYMENT_INS_LOAN
AUTO_SOLD_COLLATERAL_INS_LOAN the liquidation for borrowing asset(INS loan)
AUTO_BUY_LIABILITY_INS_LOAN the liquidation for borrowing asset(INS loan)
AUTO_PRINCIPLE_REPAYMENT_INS_LOAN
AUTO_INTEREST_REPAYMENT_INS_LOAN
TRANSFER_IN_INS_LOAN Transfer In when in the liquidation of OTC loan
TRANSFER_OUT_INS_LOAN Transfer Out when in the liquidation of OTC loan
SPOT_REPAYMENT_SELL One-click repayment currency sell
SPOT_REPAYMENT_BUY One-click repayment currency buy
TOKENS_SUBSCRIPTION Spot leverage token subscription
TOKENS_REDEMPTION Spot leverage token redemption
AUTO_DEDUCTION Asset auto deducted by system (roll back)
FLEXIBLE_STAKING_SUBSCRIPTION Byfi flexible stake subscription
FLEXIBLE_STAKING_REDEMPTION Byfi flexible stake redemption
FIXED_STAKING_SUBSCRIPTION Byfi fixed stake subscription
FLEXIBLE_STAKING_REFUND Byfi flexiable stake refund
FIXED_STAKING_REFUND Byfi fixed stake refund
PREMARKET_TRANSFER_OUT
PREMARKET_DELIVERY_SELL_NEW_COIN
PREMARKET_DELIVERY_BUY_NEW_COIN
PREMARKET_DELIVERY_PLEDGE_PAY_SELLER
PREMARKET_DELIVERY_PLEDGE_BACK
PREMARKET_ROLLBACK_PLEDGE_BACK
PREMARKET_ROLLBACK_PLEDGE_PENALTY_TO_BUYER
CUSTODY_NETWORK_FEE fireblocks business
CUSTODY_SETTLE_FEE fireblocks business
CUSTODY_LOCK fireblocks / copper business
CUSTODY_UNLOCK fireblocks business
CUSTODY_UNLOCK_REFUND fireblocks business
LOANS_BORROW_FUNDS crypto loan
LOANS_PLEDGE_ASSET crypto loan repayment
BONUS_TRANSFER_IN
BONUS_TRANSFER_OUT
PEF_TRANSFER_IN
PEF_TRANSFER_OUT
PEF_PROFIT_SHARE
ONCHAINEARN_SUBSCRIPTION tranfer out for on chain earn
ONCHAINEARN_REDEMPTION tranfer in for on chain earn
ONCHAINEARN_REFUND tranfer in for on chain earn failed
STRUCTURE_PRODUCT_SUBSCRIPTION tranfer out for structure product
STRUCTURE_PRODUCT_REFUND tranfer in for structure product
CLASSIC_WEALTH_MANAGEMENT_SUBSCRIPTION tranfer out for classic wealth management
PREMIMUM_WEALTH_MANAGEMENT_SUBSCRIPTION tranfer in for classic wealth management
PREMIMUM_WEALTH_MANAGEMENT_REFUND tranfer in for classic wealth management refund
LIQUIDITY_MINING_SUBSCRIPTION tranfer out for liquidity mining
LIQUIDITY_MINING_REFUND tranfer in for liquidity mining
PWM_SUBSCRIPTION tranfer out for PWM
PWM_REFUND tranfer in for PWM
DEFI_INVESTMENT_SUBSCRIPTION tranfer out for DEFI subscription
DEFI_INVESTMENT_REFUND transfer in for DEFI refund
DEFI_INVESTMENT_REDEMPTION tranfer in for DEFI redemption
INSTITUTION_LOAN_IN Borrowed Amount (INS Loan)
INSTITUTION_PAYBACK_PRINCIPAL_OUT Principal Repayment (INS Loan)
INSTITUTION_PAYBACK_INTEREST_OUT Interest Repayment (INS Loan)
INSTITUTION_EXCHANGE_SELL Auto Sold Collateral (INS Loan)
INSTITUTION_EXCHANGE_BUY Auto Buy Liability (INS Loan)
INSTITUTION_LIQ_PRINCIPAL_OUT Auto Principal Repayment (INS Loan)
INSTITUTION_LIQ_INTEREST_OUT Auto Interest Repayment (INS Loan)
INSTITUTION_LOAN_TRANSFER_IN Transfer in (INS Loan)
INSTITUTION_LOAN_TRANSFER_OUT Transfer out (INS Loan)
INSTITUTION_LOAN_WITHOUT_WITHDRAW Transfer out (INS Loan)
INSTITUTION_LOAN_RESERVE_IN Reserve Fund In (INS Loan)
INSTITUTION_LOAN_RESERVE_OUT Reserve Fund Out (INS Loan)
type(contract-translog)
TRANSFER_IN Assets that transferred into (inverse) derivatives wallet
TRANSFER_OUT Assets that transferred out from (inverse) derivatives wallet
TRADE
SETTLEMENT USDT / Inverse Perp funding settlement
DELIVERY Inverse Futures delivery
LIQUIDATION
ADL Auto-Deleveraging
AIRDROP
BONUS Bonus claimed
BONUS_RECOLLECT Bonus expired
FEE_REFUND Trading fee refunded
CURRENCY_BUY Currency convert
CURRENCY_SELL Currency convert
AUTO_DEDUCTION Asset auto deducted by system (roll back)
Others
unifiedMarginStatus
1 Classic account
3 Unified trading account 1.0
4 Unified trading account 1.0 (pro version)
5 Unified trading account 2.0
6 Unified trading account 2.0 (pro version)
ltStatus
1 LT can be purchased and redeemed
2 LT can be purchased, but not redeemed
3 LT can be redeemed, but not purchased
4 LT cannot be purchased nor redeemed
5 Adjusting position
convertAccountType
Check the value of unifiedMarginStatus

UTA2.0
eb_convert_uta Unified Trading Account
eb_convert_funding Funding Account
UTA1.0
eb_convert_inverse Inverse Derivatives Account (no USDT in this wallet))
eb_convert_uta Unified Trading Account
eb_convert_funding Funding Account
Classic account
Also known as the "standard account"

eb_convert_spot Spot Account
eb_convert_contract Derivatives Account (contain USDT in this wallet)
eb_convert_funding Funding Account
symbol
USDT Perpetual:

BTCUSDT
ETHUSDT
USDT Futures:

BTCUSDT-21FEB25
ETHUSDT-14FEB25
The types of USDT Futures contracts offered by Bybit include: Weekly, Bi-Weekly, Tri-Weekly, Monthly, Bi-Monthly, Quarterly, Bi-Quarterly, Tri-Quarterly
USDC Perpetual:

BTCPERP
ETHPERP
USDC Futures:

BTC-24MAR23
Inverse Perpetual:

BTCUSD
ETHUSD
Inverse Futures:

BTCUSDH23 H: First quarter; 23: 2023
BTCUSDM23 M: Second quarter; 23: 2023
BTCUSDU23 U: Third quarter; 23: 2023
BTCUSDZ23 Z: Fourth quarter; 23: 2023
Spot:

BTCUSDT
ETHUSDC
Option:

BTC-13FEB25-89000-P-USDT USDT Option
ETH-28FEB25-2800-C USDC Option
vipLevel
No VIP
VIP-1
VIP-2
VIP-3
VIP-4
VIP-5
VIP-Supreme
PRO-1
PRO-2
PRO-3
PRO-4
PRO-5
PRO-6
adlRankIndicator
0 default value of empty position
1
2
3
4
5
smpType
default: None
CancelMaker
CancelTaker
CancelBoth
extraFees.feeType
UNKNOWN
TAX   vernment tax. Only for Indonesian site
CFX Indonesian foreign exchange tax. Only for Indonesian site
WHT EU withholding tax. Only for EU site
GST Indian GST tax. Only for kyc=Indian users
VAT ARE VAT tax. Only for kyc=ARE users
extraFees.subFeeType
UNKNOWN
TAX_PNN Tax fee, fiat currency to digital currency. Only for Indonesian site
TAX_PPH Tax fee, digital currency to fiat currency. Only for Indonesian site
CFX_FIEE CFX fee, fiat currency to digital currency. Only for Indonesian site
AUT_WITHHOLDING_TAX EU site withholding tax. Only for EU site
IND_GST Indian GST tax. Only for kyc=Indian users
ARE_VAT ARE VAT tax. Only for kyc=ARE users
state
scheduled
on  ing
completed
canceled
serviceTypes
1 Trading service
2 Trading service via http request
3 Trading service via websocket
4 Private websocket stream
5 Market data service
product
1 Futures
2 Spot
3 Option
4 Spread
maintainType
1 Planned maintenance
2 Temporary maintenance
3 Incident
env
1 Production
2 Production Demo service
bizType
SPOT
DERIVATIVES
OPTIONS
msg
API limit updated successfully
Requested limit exceeds maximum allowed per user
No permission to operate these UIDs
API cap configuration not found
API cap configuration not found for bizType
Requested limit would exceed institutional quota
Spot Fee Currency Instruction
with the example of BTCUSDT:

Is makerFeeRate positive?
TRUE
Side = Buy -> base currency (BTC)
Side = Sell -> quote currency (USDT)
FALSE
IsMakerOrder = TRUE
Side = Buy -> quote currency (USDT)
Side = Sell -> base currency (BTC)
IsMakerOrder = FALSE
Side = Buy -> base currency (BTC)
Side = Sell -> quote currency (USDT)


Error Codes
HTTP Code
Code	Description
400	Bad request. Need to send the request with GET / POST (must be capitalized)
401	Invalid request. 1. Need to use the correct key to access; 2. Need to put authentication params in the request header
403	Forbidden request. Possible causes: 1. IP rate limit breached; 2. You send GET request with an empty json body; 3. You are using U.S IP
404	Cannot find path. Possible causes: 1. Wrong path; 2. Cate  ry value does not match account mode
429	System level frequency protection. Please retry when encounter this
WS OE General code
Code	Description
10404	1. op type is not found; 2. cate  ry is not correct/supported
10429	System level frequency protection
10003	Too many sessions under the same UID
10016	1. internal server error; 2. Service is restarting
10019	ws trade service is restarting, do not accept new request, but the request in the process is not affected. You can build new connection to be routed to normal service
20003	Too frequent requests under the same session
20006	reqId is duplicated
UTA
Code	Description
0	OK
-1	request expired: o@0, now[] diff[]
429	The trading service is experiencing a high server load. Please retry if you encounter this issue.
-2015	(Spot) Your api key has expired
33004	(Derivatives) Your api key has expired
10000	Server Timeout
10001	Request parameter error
10002	The request time exceeds the time window range.
10003	API key is invalid. Check whether the key and domain are matched, there are 4 env: mainnet, testnet, mainnet-demo, testnet-demo
10004	Error sign, please check your signature generation al  rithm.
10005	Permission denied, please check your API key permissions.
10006	Too many visits. Exceeded the API Rate Limit.
10007	User authentication failed.
10008	Common banned, please check your account mode
10009	IP has been banned.
10010	Unmatched IP, please check your API key's bound IP addresses.
10014	Invalid duplicate request.
10016	Server error.
10017	Route not found.
10018	Exceeded the IP Rate Limit.
10024	Compliance rules triggered
10027	Transactions are banned.
10029	The requested symbol is invalid, please check symbol whitelist
10028	The API can only be accessed by unified account users.
30133	OTC loan: The symbol you select for USDT Perpetual is not allowed by Institutional Lending
30134	OTC loan: The symbol you select for USDC Contract is not allowed by Institutional Lending
30135	The leverage you select for USDT Perpetual trading cannot exceed the maximum leverage allowed by Institutional Lending.
30136	The leverage you select for USDC Perpetual or Futures trading cannot exceed the maximum leverage allowed by Institutional Lending.
40004	the order is modified during the process of replacing , please check the order status again
100028	The API cannot be accessed by unified account users.
110001	Order does not exist
110003	Order price exceeds the allowable range.
110004	Wallet balance is insufficient
110005	position status error
110006	The assets are estimated to be unable to cover the position margin
110007	Available balance is insufficient
110008	The order has been completed or cancelled.
110009	The number of stop orders exceeds the maximum allowable limit
110010	The order has been cancelled
110011	Liquidation will be triggered immediately by this adjustment
110012	Insufficient available balance.
110013	Cannot set leverage due to risk limit level.
110014	Insufficient available balance to add additional margin.
110015	The position is in cross margin mode.
110016	The quantity of contracts requested exceeds the risk limit, please adjust your risk limit level before trying again
110017	Reduce-only rule not satisfied
110018	User ID is illegal.
110019	Order ID is illegal.
110020	Not allowed to have more than 500 active orders.
110021	Not allowed to exceeded position limits due to Open Interest.
110022	Quantity has been restricted and orders cannot be modified to increase the quantity.
110023	Currently you can only reduce your position on this contract. please check our announcement or contact customer service for details.
110024	You have an existing position, so the position mode cannot be switched.
110025	Position mode has not been modified.
110026	Cross/isolated margin mode has not been modified.
110027	Margin has not been modified.
110028	You have existing open orders, so the position mode cannot be switched.
110029	Hedge mode is not supported for this symbol.
110030	Duplicate orderId
110031	Non-existing risk limit info, please check the risk limit rules.
110032	Order is illegal
110033	You can't set margin without an open position
110034	There is no net position
110035	Cancellation of orders was not completed before liquidation
110036	You are not allowed to change leverage due to cross margin mode.
110037	User setting list does not have this symbol
110038	You are not allowed to change leverage due to portfolio margin mode.
110039	Maintenance margin rate is too high. This may trigger liquidation.
110040	The order will trigger a forced liquidation, please re-submit the order.
110041	Skip liquidation is not allowed when a position or maker order exists
110042	Currently,due to pre-delivery status, you can only reduce your position on this contract.
110043	Set leverage has not been modified.
110044	Available margin is insufficient.
110045	Wallet balance is insufficient.
110046	Liquidation will be triggered immediately by this adjustment.
110047	Risk limit cannot be adjusted due to insufficient available margin.
110048	Risk limit cannot be adjusted as the current/expected position value exceeds the revised risk limit.
110049	Tick notes can only be numbers
110050	Invalid coin
110051	The user's available balance cannot cover the lowest price of the current market
110052	Your available balance is insufficient to set the price
110053	The user's available balance cannot cover the current market price and upper limit price
110054	This position has at least one take profit link order, so the take profit and stop loss mode cannot be switched
110055	This position has at least one stop loss link order, so the take profit and stop loss mode cannot be switched
110056	This position has at least one trailing stop link order, so the take profit and stop loss mode cannot be switched
110057	Conditional order or limit order contains TP/SL related params
110058	You can't set take profit and stop loss due to insufficient size of remaining position size.
110059	Not allowed to modify the TP/SL of a partially filled open order
110060	Under full TP/SL mode, it is not allowed to modify TP/SL
110061	Not allowed to have more than 20 TP/SLs under Partial tpSlMode
110062	There is no MMP information of the institution found.
110063	Settlement in progress! {{key0}} not available for trading.
110064	The modified contract quantity cannot be less than or equal to the filled quantity.
110065	MMP hasn't yet been enabled for your account. Please contact your BD manager.
110066	Trading is currently not allowed.
110067	Unified account is not supported.
110068	Leveraged trading is not allowed.
110069	Ins lending customer is not allowed to trade.
110070	ETP symbols cannot be traded.
110071	Sorry, we're revamping the Unified Margin Account! Currently, new upgrades are not supported. If you have any questions, please contact our 24/7 customer support.
110072	OrderLinkedID is duplicate
110073	Set margin mode failed
110074	This contract is not live
110075	RiskId not modified
110076	Only isolated mode can set auto-add-margin
110077	Pm mode cannot support
110078	Added margin more than max can reduce margin
110079	The order is processing and can not be operated, please try again later
110080	Operations Restriction: The current LTV ratio of your Institutional Lending has hit the liquidation threshold. Assets in your account are being liquidated (trade/risk limit/leverage)
110082	You cannot lift Reduce-Only restrictions, as no Reduce-Only restrictions are applied to your position
110083	Reduce-Only restrictions must be lifted for both Long and Short positions at the same time
110085	The risk limit and margin ratio for this contract has been updated, please select a supported risk limit and place your order again
110086	Current order leverage exceeds the maximum available for your current Risk Limit tier. Please lower leverage before placing an order
110087	Leverage for Perpetual or Futures contracts cannot exceed the maximum allowed for your Institutional loan
110088	Please Upgrade to UTA to trade
110089	Exceeds the maximum risk limit level
110090	Order placement failed as your position may exceed the max limit. Please adjust your leverage to {{leverage}} or below to increase the max. position limit
110092	expect Rising, but trigger_price[XXXXX] <= current[XXXXX]??laste
110093	expect Falling, but trigger_price[XXXXX] >= current[XXXXX]??last
110094	Order notional value below the lower limit
110095	You cannot create, modify or cancel Pre-Market Perpetual orders during the Call Auction.
110096	Pre-Market Perpetual Trading does not support Portfolio Margin mode.
110097	Non-UTA users cannot access Pre-Market Perpetual Trading. To place, modify or cancel Pre-Market Perpetual orders, please upgrade your Standard Account to UTA.
110098	Only   od-Till-Canceled (GTC) orders are supported during Call Auction.
110099	You cannot create TP/SL orders during the Call Auction for Pre-Market Perpetuals.
110100	You cannot place, modify, or cancel Pre-Market Perpetual orders when you are in Demo Trading.
110101	Trading inverse contracts under Cross and Portfolio modes requires enabling the settlement asset as collateral.
110102	The user does not support trading Inverse contracts - copy trading pro, Ins loan account are not supported
110103	Only Post-Only orders are available at this stage
110104	The LTV for ins Loan has exceeded the limit, and opening inverse contracts is prohibited
110105	The LTV for ins Loan has exceeded the limit, and trading inverse contracts is prohibited
110106	Restrictions on Ins Loan; inverse contracts are not on the whitelist and are not allowed for trading
110107	Restrictions on ins Loan; leverage exceeding the limit for inverse contracts is not allowed.
110108	Allowable range: 5 to 2000 tick size
110109	Allowable range: 0.05% to 1%
110110	Spread trading is not available in isolated margin trading mode
110111	To access spread trading, upgrade to the latest version of UTA
110112	Spread trading is not available for Copy Trading
110113	Spread trading is not available in hedge mode
110114	You have a Spread trading order in progress. Please try again later
110115	The cancellation of a combo single-leg order can only be done by canceling the combo order
110116	The entry price of a single leg, derived from the combo order price, exceeds the limit price
110117	The modification of a combo single-leg order can only be done by modifying the combo order
110118	Unable to retrieve a pruce of the market order due to low liquidity
110119	Order failed. RPI orders are restricted to approved market makers only
110120	Order price cannot be smaller than xxxx, the price limitation
110121	Order price cannot be higher than xxxx, the price limitation
170346	Settle coin is not a collateral coin, cannot trade
170360	symbol[XXXX] cannot trade. Used for spread trading in particular when collateral is not turned on
181017	OrderStatus must be final status
182100	Compulsory closing of positions, no repayment allowed
182101	Failed repayment, insufficient collateral balance
182102	Failed repayment, there are no liabilities in the current currency
182103	Institutional lending users are not supported
182108	Switching failed, margin verification failed, please re-adjust the currency status
182110	Failed to switch
182111	The requested currency has a non guaranteed   ld currency or does not support switching status currencies
182112	Duplicate currency, please re-adjust
3100181	UID can not be null
3100197	Temporary banned due to the upgrade to UTA
3200316	USDC Options Trading Restriction: The current LTV ratio for your Institutional Lending has reached the maximum allowable amount for USDC Options trading.
3200317	USDC Options Open Position Restriction: The current LTV ratio for your Institutional Lending has reached the maximum allowable amount for opening USDC Options positions.
3100326	BaseCoin is required
3200403	isolated margin can not create order
3200419	Unable to switch to Portfolio margin due to active pre-market Perpetual orders and positions
3200320	Operations Restriction: The current LTV ratio of your Institutional Lending has hit the liquidation threshold. Assets in your account are being liquidated. (margin mode or spot leverage)
3400208	You have unclosed hedge mode or isolated mode USDT perpetual positions
3400209	You have USDT perpetual positions, so upgrading is prohibited for 10 minutes before and after the hour every hour
3400210	The risk rate of your Derivatives account is too high
3400211	Once upgraded, the estimated risk rate will be too high
3400212	You have USDC perpetual positions or Options positions, so upgrading is prohibited for 10 minutes before and after the hour every hour
3400213	The risk rate of your USDC Derivatives account is too high
3400052	You have uncancelled USDC perpetual orders
3400053	You have uncancelled Options orders
3400054	You have uncancelled USDT perpetual orders
3400214	Server error, please try again later
3400071	The net asset is not satisfied
3401010	Cannot switch to PM mode (for copy trading master trader)
3400139	The total value of your positions and orders has exceeded the risk limit for a Perpetual or Futures contract
34040	Not modified. Indicates you already set this TP/SL value or you didn't pass a required parameter
500010	The subaccount specified does not belong to the parent account
500011	The Uid 592334 provided is not associated with a Unified Trading Account
Spot Trade
Code	Description
170001	Internal error.
170005	Too many new orders; current limit is %s orders per %s.
170007	Timeout waiting for response from backend server.
170010	Purchase failed: Exceed the maximum position limit of leveraged tokens, the current available limit is %s USDT
170011	"Purchase failed: Exceed the maximum position limit of innovation tokens,
170019	the current available limit is ''{{.replaceKey0}}'' USDT"
170031	The feature has been suspended
170032	Network error. Please try again later
170033	margin Insufficient account balance
170034	Liability over flow in spot leverage trade!
170035	Submitted to the system for processing!
170036	You haven't enabled Cross Margin Trading yet. To do so, please head to the PC trading site or the Bybit app
170037	Cross Margin Trading not yet supported by the selected coin
170105	Parameter '%s' was empty.
170115	Invalid timeInForce.
170116	Invalid orderType.
170117	Invalid side.
170121	Invalid symbol.
170124	Order amount too large.
170130	Data sent for paramter '%s' is not valid.
170131	Balance insufficient
170132	Order price too high.
170133	Order price lower than the minimum.
170134	Order price decimal too long.
170135	Order quantity too large.
170136	Order quantity lower than the minimum.
170137	Order volume decimal too long
170139	Order has been filled.
170140	Order value exceeded lower limit
170141	Duplicate clientOrderId
170142	Order has been cancelled
170143	Cannot be found on order book
170144	Order has been locked
170145	This order type does not support cancellation
170146	Order creation timeout
170147	Order cancellation timeout
170148	Market order amount decimal too long
170149	Create order failed
170150	Cancel order failed
170151	The trading pair is not open yet
170157	The trading pair is not available for api trading
170159	Market Order is not supported within the first %s minutes of newly launched pairs due to risk control.
170190	Cancel order has been finished
170191	Can not cancel order, please try again later
170192	Order price cannot be higher than %s .
170193	Buy order price cannot be higher than %s.
170194	Sell order price cannot be lower than %s.
170195	Please note that your order may not be filled. ETP buy order price deviates from risk control
170196	Please note that your order may not be filled. ETP sell order price deviates from risk control
170197	Your order quantity to buy is too large. The filled price may deviate significantly from the market price. Please try again
170198	Your order quantity to sell is too large. The filled price may deviate significantly from the market price. Please try again
170199	Your order quantity to buy is too large. The filled price may deviate significantly from the nav. Please try again.
170200	Your order quantity to sell is too large. The filled price may deviate significantly from the nav. Please try again.
170201	Invalid orderFilter parameter
170202	Please enter the TP/SL price.
170203	trigger price cannot be higher than 110% price.
170204	trigger price cannot be lower than 90% of qty.
170206	Stop_limit Order is not supported within the first 5 minutes of newly launched pairs
170207	The loan amount of the platform is not enough.
170210	New order rejected.
170212	Cancel order request processing
170213	Order does not exist.
170215	Spot Trading (Buy) Restriction: The current LTV ratio of your institutional lending has reached the maximum allowable amount for buy orders
170216	The leverage you select for Spot Trading cannot exceed the maximum leverage allowed by Institutional Lending
170217	Only LIMIT-MAKER order is supported for the current pair.
170218	The LIMIT-MAKER order is rejected due to invalid price.
170219	UID {{xxx}} is not available to this feature
170220	Spot Trading Restriction: The current LTV ratio of your institutional lending has reached the maximum allowable amount for Spot trading
170221	This coin does not exist.
170222	Too many requests in this time frame.
170223	Your Spot Account with Institutional Lending triggers an alert or liquidation.
170224	You're not a user of the Innovation Zone.
170226	Your Spot Account for Margin Trading is being liquidated.
170227	This feature is not supported.
170228	The purchase amount of each order exceeds the estimated maximum purchase amount.
170229	The sell quantity per order exceeds the estimated maximum sell quantity.
170230	Operations Restriction: Due to the deactivation of Margin Trading for institutional loan
170234	System Error
170241	To proceed with trading, users must read through and confirm that they fully understand the project's risk disclosure document. For App users, please update your Bybit App to version 4.16.0 to process.
170310	Order modification timeout
170311	Order modification failed
170312	The current order does not support modification
170313	The modified contract quantity cannot be less than to the filled quantity
170341	Request order quantity exceeds maximum limit
170344	Symbol is not supported on Margin Trading
170348	Please    to (https://www.bybit-tr.com) to proceed.
170355	RPI orders are restricted to approved market makers only
170358	The current site does not support ETP
170359	TThe current site does not support leveraged trading
170709	OTC loan: The select trading pair is not in the whitelist pair
170810	Cannot exceed maximum of 500 conditional, TP/SL and active orders.
Spot Leverage Token
Code	Description
175000	The serialNum is already in use.
175001	Daily purchase limit has been exceeded. Please try again later.
175002	There's a large number of purchase orders. Please try again later.
175003	Insufficient available balance. Please make a deposit and try again.
175004	Daily redemption limit has been exceeded. Please try again later.
175005	There's a large number of redemption orders. Please try again later.
175006	Insufficient available balance. Please make a deposit and try again.
175007	Order not found.
175008	Purchase period hasn't started yet.
175009	Purchase amount has exceeded the upper limit.
175010	You haven't passed the quiz yet! To purchase and/or redeem an LT, please complete the quiz first.
175012	Redemption period hasn't started yet.
175013	Redemption amount has exceeded the upper limit.
175014	Purchase of the LT has been temporarily suspended.
175015	Redemption of the LT has been temporarily suspended.
175016	Invalid format. Please check the length and numeric precision.
175017	Failed to place order：Exceed the maximum position limit of leveraged tokens, the current available limit is XXXX USDT
175027	Subscriptions and redemptions are temporarily unavailable while account upgrade is in progress
Spot Margin Trade
Code	Description
176002	Query user account info error. Confirm that if you have completed quiz in GUI
176003	Query user loan history error
176004	Query order history start time exceeds end time
176005	Failed to borrow
176006	Repayment Failed
176007	User not found
176008	You haven't enabled Cross Margin Trading yet. To do so, please head to the PC trading site
176009	You haven't enabled Cross Margin Trading yet. Confirm that if you have turned on margin trade
176010	Failed to locate the coins to borrow
176011	Cross Margin Trading not yet supported by the selected coin
176012	Pair not available
176013	Cross Margin Trading not yet supported by the selected pair
176014	Repeated repayment requests
176015	Insufficient available balance
176016	No repayment required
176017	Repayment amount has exceeded the total liability
176018	Settlement in progress
176019	Liquidation in progress
176020	Failed to locate repayment history
176021	Repeated borrowing requests
176022	Coins to borrow not generally available yet
176023	Pair to borrow not generally available yet
176024	Invalid user status
176025	Amount to borrow cannot be lower than the min. amount to borrow (per transaction)
176026	Amount to borrow cannot be larger than the max. amount to borrow (per transaction)
176027	Amount to borrow cannot be higher than the max. amount to borrow per user
176028	Amount to borrow has exceeded Bybit's max. amount to borrow
176029	Amount to borrow has exceeded the user's estimated max. amount to borrow
176030	Query user loan info error
176031	Number of decimals for borrow amount has exceeded the maximum precision
176034	The leverage ratio is out of range
176035	Failed to close the leverage switch during liquidation
176036	Failed to adjust leverage switch during forced liquidation
176037	For non-unified transaction users, the operation failed
176038	The spot leverage is closed and the current operation is not allowed
176039	Borrowing, current operation is not allowed
176040	There is a spot leverage order, and the adjustment of the leverage switch failed!
176132	Number of decimals for repay amount has exceeded the maximum precision
176133	Liquidation may be triggered! Please adjust your transaction amount and try again
176134	Account has been upgraded (upgrading) to UTA
176135	Failed to get bond data
176136	Failed to get borrow data
176137	Failed to switch user status
176138	You need to repay all your debts before closing your disabling cross margin account
176139	Sorry, you are not eligible to enable cross margin, as you have already enabled OTC lending
176201	Account exception. Check if the UID is bound to an institutional loan
182021	Cannot enable spot margin while in isolated margin mode. Please switch to cross margin mode or portfolio margin mode to trade spot with margin.
182104	This action could not be completed as your Unified Margin Account's IM/MM utilization rate has exceeded the threshold
182105	Adjustment failed, user is upgrading
182106	Adjustment failed, user forced liquidation in progress.
182107	Adjustment failed, Maintenance Margin Rate too high
Asset
Code	Description
131001	openapi svc error
131002	Parameter error
131002	Withdraw address chain or destination tag are not equal
131003	Internal error
131004	KYC needed
131065	Your KYC information is incomplete, please    to the KYC information page of the web or app to complete the information. kyc=India client may encounter this
131066	This address does not support withdrawals for the time being. Please switch to another address for withdrawing
131067	Travel rule verification failed, please contact the target exchange. Travel rule for KR user
131068	Travel rule information is insufficient, please provide additional details. Travel rule for KR user
131069	Unable to withdraw to the receipt, please contact the target the exchange. Travel rule for KR user
131070	The recipient's name is mismatched with the targeted exchange. Travel rule for KR user
131071	The recipient has not under  ne KYC verification. Travel rule for KR user
131072	Your withdrawal currency is not supported by the target exchange. Travel rule for KR user
131073	Your withdrawal address has not been included in the target exchange. Travel rule for KR user
131074	Beneficiary info is required, please refer to the latest api document. Travel rule for KR user
131075	InternalAddressCannotBeYourself
131076	internal transfer not support subaccounts
131077	receive user not exist
131078	receive user deposit has been banned
131079	receive user need kyc
131080	User left retry times is zero
131081	Do not input memo/tag,please.
131082	Do not repeat the request
131083	Withdraw only allowed from address book
131084	Withdraw failed because of Uta Upgrading
131085	Withdrawal amount is greater than your availale balance (the deplayed withdrawal is triggered)
131086	Withdrawal amount exceeds risk limit (the risk limit of margin trade is triggered)
131087	your current account spot risk level is too high, withdrawal is prohibited, please adjust and try again
131088	The withdrawal amount exceeds the remaining withdrawal limit of your identity verification level. The current available amount for withdrawal : %s
131089	User sensitive operation, withdrawal is prohibited within 24 hours
131090	User withdraw has been banned
131091	Blocked login status does not allow withdrawals
131092	User status is abnormal
131093	The withdrawal address is not in the whitelist
131094	UserId is not in the whitelist
131095	Withdrawl amount exceeds the 24 hour platform limit
131096	Withdraw amount does not satify the lower limit or upper limit
131097	Withdrawal of this currency has been closed
131098	Withdrawal currently is not availble from new address
131099	Hot wallet status can cancel the withdraw
131200	Service error
131201	Internal error
131202	Invalid memberId
131203	Request parameter error
131204	Account info error
131205	Query transfer error
131206	cannot be transfer
131207	Account not exist
131208	Forbid transfer
131209	Get subMember relation error
131210	Amount accuracy error
131211	fromAccountType can't be the same as toAccountType
131212	Insufficient balance
131213	TransferLTV check error
131214	TransferId exist
131215	Amount error
131216	Query balance error
131217	Risk check error
131227	subaccount do not have universal transfer permission
131228	your balance is not enough. Please check transfer safe amount
131229	Due to compliance requirements, the current currency is not allowed to transfer
131230	The system is busy, please try again later
131231	Transfers into this account are not supported
131232	Transfers out this account are not supported
131233	can not transfer the coin that not supported for islamic account
140001	Switching the PM spot hedging switch is not allowed in non PM mode
140002	Institutional lending users do not support PM spot hedging
140003	You have position(s) being liquidated, please try again later.
140004	Operations Restriction: The current LTV ratio of your Institutional Loan has hit the liquidation threshold. Assets in your account are being liquidated.
140005	Risk level after switching modes exceeds threshold
141004	sub member is not normal
141025	This subaccount has assets and cannot be deleted
181000	cate  ry is null
181001	cate  ry only support linear or option or spot.
181002	symbol is null.
181003	side is null.
181004	side only support Buy or Sell.
181005	orderStatus is wrong
181006	startTime is not number
181007	endTime is not number
181008	Parameter startTime and endTime are both needed
181009	Parameter startTime needs to be smaller than endTime
181010	The time range between startTime and endTime cannot exceed 7 days
181011	limit is not a number
181012	symbol not exist
181013	Only support settleCoin: usdc
181014	Classic account is not supported
181018	Invalid expDate.
181019	Parameter expDate can't be earlier than 2 years
182000	symbol related quote price is null
182200	Please upgrade UTA first.
182201	You must enter 2 time parameters.
182202	The start time must be less than the end time
182203	Please enter valid characters
182204	Coin does not exist
182205	User level does not exist
700000	accountType/quoteTxId cannot be null
700001	quote fail:no dealer can used
700004	order does not exist
700007	Large Amount Limit
700012	UTA upgrading, don't allow to apply for quote
Crypto Loan (New)
Code	Description
148001	This currency is not supported for flexible savings.
148002	The entered amount is below the minimum borrowable amount.
148003	Exceeds the allowed decimal precision for this currency.
148004	This currency cannot be used as collateral.
148005	Exceeds the allowed decimal precision for this collateral currency.
148006	The amount of collateral exceeds the upper limit of the platform.
148007	Borrow amount cannot be negative.
148008	Collateral amount cannot be negative.
148009	LTV exceeds the risk threshold.
148010	Insufficient available quota.
148011	Insufficient balance in the funding pool .
148012	Insufficient collateral amount.
148013	Non-borrowing users cannot adjust collateral.
148014	This currency is not supported.
148015	Loan term exceeds the allowed range.
148016	The specified lending rate is not supported.
148017	The interest rate exceeds the allowed decimal precision.
148018	Exceeded the maximum number of open orders.
148019	The system is busy, please try again later.
148020	Insufficient platform lending quota.
148021	Operation conflict detected. Please try again later.
148022	Insufficient assets for lending.
148023	Loan order not found.
148024	Loan cancellation failed: the order may have been completed or has an invalid amount.
148025	Lending order cancellation failed: the order may have been completed or has an invalid amount.
148026	Failed to create repayment. Please try again later.
148027	No active loan found for this account. Operation not allowed.
148028	Repayment amount exceeds the supported precision for the currency.
148029	Insufficient balance in the repayment account.
148030	Deposit order not found.
148031	Operation not allowed during liquidation.
148032	No outstanding debt. Repayment is not allowed.
148033	This loan order cannot be repaid.
148034	Please wait and try again later.
148035	Please wait and try again later.
148036	Failed to adjust collateral amount. Please try again later.
148037	Insufficient assets or adjustment amount exceeds the maximum allowed.
148038	Repayment amount cannot exceed the debt amount of the position.
148039	Duplicate collateral assets detected. Please review and resubmit.
Crypto Loan (legacy)
Code	Description
177002	Server is busy, please wait and try again
177003	Illegal characters found in a parameter
177004	Precision is over the maximum defined for this asset
177005	Order does not exist
177006	We don't have this asset
177007	Your borrow amount has exceed maximum borrow amount
177008	Borrow is banned for this asset
177009	Borrow amount is less than minimum borrow amount
177010	Repay amount exceeds borrow amount
177011	Balance is not enough
177012	The system doesn't have enough asset now
177013	adjustment amount exceeds minimum collateral amount
177014	Individual loan quota reached
177015	Collateral amount has reached the limit. Please reduce your collateral amount or try with other collaterals
177016	Minimum collateral amount is not enough
177017	This coin cannot be used as collateral
177018	duplicate request
177019	Your input param is invalid
177020	The account does not support the asset
177021	Repayment failed
Institutional Loan
Code	Description
3777002	UID cannot be bound repeatedly.
3777003	UID cannot be unbound because the UID has not been bound to a risk unit.
3777004	The main UID of the risk unit cannot be unbound.
3777005	You have unsettled lending or borrowing orders. Please try again later.
3777006	UID cannot be bound, please try again with a different UID."
3777007	UID cannot be bound, please upgrade to UTA Pro."
3777012	Your request is currently being processed. Please wait and try again later
3777027	UID cannot be bound, leveraged trading closure failed.
3777029	You currently have orders for pre-market trading that can’t be bind UIDs
3777030	This account has activated copyPro and cannot bind uid
Exchange Broker
Code	Description
3500402	Parameter verification failed for 'limit'.
3500403	Only available to exchange broker main-account
3500404	Invalid Cursor
3500405	Parameter "startTime" and "endTime" need to be input in pairs.
3500406	Out of query time range.
3500407	Parameter begin and end need to be input in pairs.
Reward
Code	Description
400001	invalid parameter
400101	The voucher was recycled
400102	The voucher has exceeded the redemption date (expired)
400103	The voucher is not available for redemption
400105	Budget exceeded
403001	Account rejected, check if the input accountId valid, account banned, or kyc issue
404001	resource not found
404011	Insufficient inventory
409011	VIP level limit
500001	Internal server error
Earn
Code	Description
180001	Invalid parameter
180002	Invalid coin
180003	User banned
180004	Site not allowed. Only users from Bybit global site can access
180005	Compliance wallet not reach
180006	Validation failed
180007	Product not available
180008	Invalid Product
180009	product is forbidden
180010	User not allowed
180011	User not VIP
180012	Purchase share is invalid
180013	Stake over maximum share
180014	Redeem share invlaid
180015	Products share not enough
180016	Balance not enough
180017	Invalid risk user
180018	internal error
180019	empty order link id
User
Code	Description
81007	Bybit Europe is not supported create API Key
20096	need KYC authentication
Set api rate limit
Code	Description
3500002	Current user is not an institutional user
3500153	No permission to operate these UIDs
3500153	You do not have permission to query other UIDs

