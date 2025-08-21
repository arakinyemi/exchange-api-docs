### Deposit Info Channel

A push notification is triggered when a deposit is initiated or the deposit status changes.

**URL Path**: `/ws/v5/business` (required login)

Request Example
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

Request Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| id | String | No | Unique identifier of the message |
| op | String | Yes | Operation: subscribe, unsubscribe |
| args | Array | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name: deposit-info |
| > ccy | String | No | Currency, e.g. BTC |

Response Example
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

Failure Response Example

Response parameters

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

Push Data Example

Push data parameters

| Parameter       | Type        | Description                                                                                     |
|-----------------|-------------|-------------------------------------------------------------------------------------------------|
| arg             | Object      | Successfully subscribed channel                                                                 |
| > channel       | String      | Channel name (e.g. deposit-info)                                                                |
| > uid           | String      | User Identifier                                                                                 |
| > ccy           | String      | Currency, e.g. BTC                                                                             |
| data            | Array       | Array of objects, subscribed data                                                               |

Subscribed data parameters

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

Withdrawal info channel

- A push notification is triggered when a withdrawal is initiated or withdrawal status changes.
- Supports subscriptions for accounts.
- Master account subscription receives push for master and sub-account withdrawals.
- Sub-account subscription receives only sub-account withdrawal info.

URL Path

`/ws/v5/business` (required login)

---

Request Example

Request Parameters

| Parameter | Type         | Required | Description                                                                              |
|-----------|--------------|----------|------------------------------------------------------------------------------------------|
| id        | String       | No       | Unique identifier of the message. Provided by client. Returned in response for tracking. |
| op        | String       | Yes      | Operation: subscribe / unsubscribe                                                     |
| args      | Array<Object> | Yes      | List of subscribed channels                                                             |

Args object details

| Parameter | Type   | Required | Description         |
|-----------|--------|----------|---------------------|
| channel   | String | Yes      | Channel name: withdrawal-info |
| ccy       | String | No       | Currency, e.g. BTC   |

---

Response parameters

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

Push Data Example (Withdrawal info)

Push data parameters

| Parameter | Type   | Description                     |
|-----------|--------|--------------------------------|
| arg       | Object | Successfully subscribed channel |
| > channel | String | Channel name                   |
| > uid     | String | User Identifier                |
| > ccy     | String | Currency, e.g. BTC             |
| data      | Array  | Array of subscribed data       |

Subscribed data parameters

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
