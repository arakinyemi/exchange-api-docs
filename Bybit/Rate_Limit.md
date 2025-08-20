# Rate Limit

## IP Limit

### HTTP IP limit

You are allowed to send 600 requests within a 5-second window per IP by default. This limit applies to all traffic directed to api.bybit.com, api.bybick.com, and local site hostnames such as api.bybit.kz. If you encounter the error "403, access too frequent", it indicates that your IP has exceeded the allowed request frequency. In this case, you should terminate all HTTP sessions and wait for at least 10 minutes. The ban will be lifted automatically.

We do not recommend running your application at the very edge of these limits in case abnormal network activity results in an unexpected violation.

### Websocket IP limit

Do not establish more than 500 connections within a 5-minute window. This limit applies to all connections directed to stream.bybit.com as well as local site hostnames such as stream.bybit.kz.

Do not frequently connect and disconnect the connection

Do not establish more than 1,000 connections per IP for market data. The connection limits are counted separately for Spot, Linear, Inverse, and Options markets

## API Rate Limit
caution

If you receive "retCode": 10006, "retMsg": "Too many visits!" in the JSON response, you have hit the API rate limit.

The API rate limit is based on the rolling time window per second and UID. In other words, it is per second per UID. Every request to the API returns response header shown in the code panel:

X-Bapi-Limit-Status - your remaining requests for current endpoint

X-Bapi-Limit - your current limit for current endpoint

X-Bapi-Limit-Reset-Timestamp - the timestamp indicating when your request limit resets if you have exceeded your rate_limit. Otherwise, this is just the current timestamp (it may not exactly match timeNow).

Http Response Header Example
```http
▶Response Headers
Content-Type: application/json; charset=utf-8
Content-Length: 141
X-Bapi-Limit: 100
X-Bapi-Limit-Status: 99
X-Bapi-Limit-Reset-Timestamp: 1672738134824
```

### API Rate Limit Table
#### Trade

Classic account
| Method |         Path         | Classic account |        |      | upgradable |
|:------:|:--------------------:|:---------------:|:------:|------|:----------:|
|        |                      |     inverse     | linear | spot |            |
| POST   | /v5/order/create     | 10/s            |     10/s   | 20/s | Y          |
|        | /v5/order/amend      | 10/s            |    10/s    | 10/s | Y          |
|        | /v5/order/cancel     | 10/s            |     10/s   | 20/s | Y          |
|        | /v5/order/cancel-all | 10/s            |     10/s   | 20/s | Y          |
| GET    | /v5/order/realtime   | 10/s            |    10/s    | 20/s | N          |
|        | /v5/order/history    | 10/s            |    10/s    | 20/s | N          |
|        | /v5/execution/list   | 10/s            |     10/s   | 20/s | N          |

#### Position

Classic account

| Method |            Path           | Classic account |        |      | upgradable |
|:------:|:-------------------------:|-----------------|--------|------|:----------:|
|        |                           |     inverse     | linear | spot |            |
| GET    | /v5/position/list         | 10/s            |        | -    | N          |
|        | /v5/position/closed-pnl   | 10/s            |        | -    | N          |
| POST   | /v5/position/set-leverage | 10/s            |        | -    | N          |

#### Account

Classic account

| Method |                 Path                 |                      | Limit | upgradable |
|:------:|:------------------------------------:|:--------------------:|:-----:|:----------:|
| GET    | /v5/account/contract-transaction-log |                      | 10/s  | N          |
|        | /v5/account/wallet-balance           | accountType=SPOT     | 20/s  | N          |
|        |                                      | accountType=CONTRACT | 10/s  | N          |
|        | /v5/account/fee-rate                 | category=linear      | 10/s  | N          |
|        |                                      | category=spot        | 5/s   | N          |
|        |                                      | category=option      | 5/s   | N          |

#### Asset

Classic Account
| Method |                       Path                       |    Limit    | upgradable |
|:------:|:------------------------------------------------:|:-----------:|:----------:|
| GET    | /v5/asset/transfer/query-asset-info              | 60 req/min  | N          |
|        | /v5/asset/transfer/query-transfer-coin-list      | 60 req/min  | N          |
|        | /v5/asset/transfer/query-inter-transfer-list     | 60 req/min  | N          |
|        | /v5/asset/transfer/query-sub-member-list         | 60 req/min  | N          |
|        | /v5/asset/transfer/query-universal-transfer-list | 5 req/s     | N          |
|        | /v5/asset/transfer/query-account-coins-balance   | 5 req/s     | N          |
|        | /v5/asset/deposit/query-record                   | 100 req/min | N          |
|        | /v5/asset/deposit/query-sub-member-record        | 300 req/min | N          |
|        | /v5/asset/deposit/query-address                  | 300 req/min | N          |
|        | /v5/asset/deposit/query-sub-member-address       | 300 req/min | N          |
|        | /v5/asset/withdraw/query-record                  | 300 req/min | N          |
|        | /v5/asset/coin/query-info                        | 5 req/s     | N          |
|        | /v5/asset/exchange/order-record                  | 600 req/min | N          |
| POST   | /v5/asset/transfer/inter-transfer                | 60 req/min  | N          |
|        | /v5/asset/transfer/save-transfer-sub-member      | 20 req/s    | N          |
|        | /v5/asset/transfer/universal-transfer            | 5 req/s     | N          |
|        | /v5/asset/withdraw/create                        | 5 req/s     | N          |
|        | /v5/asset/withdraw/cancel                        | 60 req/min  | N          |

#### User

| Method |            Path            |   Limit  | upgradable |
|:------:|:--------------------------:|:--------:|------------|
| POST   | v5/user/create-sub-member  | 1 req/s  | N          |
|        | /v5/user/create-sub-api    | 1 req/s  | N          |
|        | /v5/user/frozen-sub-member | 5 req/s  | N          |
|        | /v5/user/update-api        | 5 req/s  | N          |
|        | /v5/user/update-sub-api    | 5 req/s  | N          |
|        | /v5/user/delete-api        | 5 req/s  | N          |
|        | /v5/user/delete-sub-api    | 5 req/s  | N          |
| GET    | /v5/user/query-sub-members | 10 req/s | N          |
|        | /v5/user/query-api         | 10 req/s | N          |
|        | /v5/user/aff-customer-info | 10 req/s | N          |

#### Spot Leverage Token

Classic Account
| Method | Path                              |   Limit  | Upgradable |
|--------|-----------------------------------|:--------:|:----------:|
| GET    | /v5/spot-lever-token/order-record | 50 req/s | N          |
| POST   | /v5/spot-lever-token/purchase     | 20 req/s | N          |
| POST   | /v5/spot-lever-token/redeem       | 20 req/s | N          |

#### Spot Margin Trade (UTA)

For now, there is no limit for endpoints under this category

---

#### Spread Trading
| Method | Path                     |   Limit  | Upgradable |
|--------|--------------------------|:--------:|:----------:|
| POST   | Create Spread Order      | 20 req/s | N          |
| POST   | Amend Spread Order       | 20 req/s | N          |
| POST   | Cancel Spread Order      | 20 req/s | N          |
| POST   | Cancel All Spread Orders | 5 req/s  | N          |
| GET    | Get Spread Open Orders   | 50 req/s | N          |
| GET    | Get Spread Order History | 50 req/s | N          |
| GET    | Get Spread Trade History | 50 req/s | N          |


### API Rate Limit Rules For VIPs
Instructions for batch endpoints

The batch order endpoint, which includes operations for creating, amending, and canceling, has its own rate limit and does not share it with single requests, e.g., let's say the rate limit of single create order endpoint is 100/s, and batch create order endpoint is 100/s, so in this case, I can place 200 linear orders in one second if I use both endpoints to place orders


When category = linear spot or inverse

API for batch create/amend/cancel order, the frequency of the API will be consistent with the current configuration, but the counting consumption will be consumed according to the actual number of orders. (Number of consumption = number of requests * number of orders included in a single request), and the configuration of business lines is independent of each other.

The batch APIs allows 1-10 orders/request. For example, if a batch order request is made once and contains 5 orders, then the request limit will consume 5.

If part of the last batch of orders requested within 1s exceeds the limit, the part that exceeds the limit will fail, and the part that does not exceed the limit will succeed. For example, in the 1 second, the remaining limit is 5, but a batch request containing 8 orders is placed at this time, then the first 5 orders will be successfully placed, and the 6-8th orders will report an error exceeding the limit, and these orders will fail.

|               |         | Unified Account |      |
|---------------|---------|:---------------:|:----:|
| Level\Product | Futures | Option          | Spot |
| Default       | 10/s    | 10/s            | 20/s |
| VIP 1         | 20/s    | 20/s            | 25/s |
| VIP 2         | 40/s    | 40/s            | 30/s |
| VIP 3         | 60/s    | 60/s            | 40/s |
| VIP 4         | 60/s    | 60/s            | 40/s |
| VIP 5         | 60/s    | 60/s            | 40/s |
| VIP Supreme   | 60/s    | 60/s            | 40/s |

### API Rate Limit Rules For PROs
UPCOMING CHANGES FOR PRO ACCOUNT

Starting August 13, 2025, Bybit will roll out a new institutional API rate limit framework designed to enhance performance for high-frequency trading clients. The new system introduces a centralized institution-level rate cap with flexible per-UID configurations, enabling greater efficiency and scalability. Refer to announcement

#### UID level:
|               |         | Unified Account |        |
|---------------|---------|-----------------|:------:|
| Level\Product | Futures | Option          | Spot   |
| PRO1          | 200/s   | 200/s           | 200/s  |
| PRO2          | 400/s   | 400/s           | 400/s  |
| PRO3          | 600/s   | 600/s           | 600/s  |
| PRO4          | 800/s   | 800/s           | 800/s  |
| PRO5          | 1000/s  | 1000/s          | 1000/s |
| PRO6          | 1200/s  | 1200/s          | 1200/s |

#### Master and subaccounts level:
|               |         | Unified Account |         |
|---------------|---------|-----------------|:-------:|
| Level\Product | Futures | Option          | Spot    |
| PRO1          | 10000/s | 10000/s         | 10000/s |
| PRO2          | 20000/s | 20000/s         | 20000/s |
| PRO3          | 30000/s | 30000/s         | 30000/s |
| PRO4          | 40000/s | 40000/s         | 40000/s |
| PRO5          | 50000/s | 50000/s         | 50000/s |
| PRO6          | 60000/s | 60000/s         | 60000/s |


# Set api rate limit
API rate limit: 50 req per second

info

If UID requesting this endpoint is a master account, uids in the input parameter must be subaccounts of the master account.

If UID requesting this endpoint is not a master account, uids in the input parameter must be the UID requesting this endpoint

UID requesting this endpoint must be an institutional user.
up to 10 uids can be submitted in each request

HTTP Request
```http
POST /v5/apilimit/set
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| list | true | array | Object |
| > uids | true | string | Multiple UIDs, separated by commas |
| > bizType | true | string | Business type |
| > limit | true | integer | api rate limit per second |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > uids | string | Multiple UIDs, separated by commas |
| > bizType | string | Business type |
| > limit | integer | api rate limit per second |
| > success | boolean | success or not |
| > msg | string | result message |

---

Request Example
```http
POST /v5/apilimit/set HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1711420489915
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
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
    "retExtinfo
": {},
    "time": 1754894296913
}
```



# Query api rate limit
API rate limit: 50 req per second

info

A master account can query api rate limit of its own and subaccounts.

A subaccount can only query its own api rate limit.

HTTP Request
```http
GET /v5/apilimit/query
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| uids | true | string | Multiple UIDs, separated by commas |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | Object |
| > uids | string | Multiple UIDs, separated by commas |
| > bizType | string | Business type |
| > limit | integer | api rate limit per second |

---

Request Example
```http
GET /v5/apilimit/query?uids=290118 HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1728460942776
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 2
```

```json
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
    "retExtinfo
": {},
    "time": 1754894341984
}
```

