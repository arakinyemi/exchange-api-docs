### Convert Trade

Execute a currency conversion trade. You should make estimate quote before convert trade.

> **Note:** Only assets in the trading account supported convert.

- **Rate Limit:** 10 requests per second
- **Rate Limit Rule:** User ID
- **Permission:** Trade
- **Trading Limit:** For the same side (buy/sell), there's a trading limit of 1 request per 5 seconds.

HTTP Request
```
POST /api/v5/asset/convert/trade
```

Request Example
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

Request Parameters
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

Response Example
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

Response Parameters
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
