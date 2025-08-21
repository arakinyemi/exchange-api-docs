### Get deposit address
Retrieve the deposit addresses of currencies, including previously-used addresses.

**Rate Limit:** 6 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

HTTP Request
```
GET /api/v5/asset/deposit-address
```

Request Example
```
GET /api/v5/asset/deposit-address?ccy=BTC
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | Yes | Currency, e.g. BTC |

Response Example
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

Response Parameters

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
