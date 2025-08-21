### Get Withdrawal History

Retrieve the withdrawal records according to the currency, withdrawal status, and time range in reverse chronological order. The 100 most recent records are returned by default.

Websocket API is also available, refer to Withdrawal info channel.

**Rate Limit**: 6 requests per second  
**Rate limit rule**: User ID  
**Permission**: Read  

HTTP Request
`GET /api/v5/asset/withdrawal-history`

Request Example

```
GET /api/v5/asset/withdrawal-history

# Query withdrawal history from 2022-06-01 to 2022-07-01
GET /api/v5/asset/withdrawal-history?ccy=BTC&after=1654041600000&before=1656633600000
```

Request Parameters

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

Response Example
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

Response Parameters

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
