### Exchange information​

```
{  
  "id": "5494febb-d167-46a2-996d-70533eb4d976",  
  "method": "exchangeInfo",  
  "params": {  
    "symbols": ["BNBBTC"]  
  }  
}
```

Query current exchange trading rules, rate limits, and symbol information.

**Weight:**
20

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `symbol` | STRING | NO | Describe a single symbol |
| `symbols` | ARRAY of STRING | Describe multiple symbols |
| `permissions` | ARRAY of STRING | Filter symbols by permissions |
| `showPermissionSets` | BOOLEAN | Controls whether the content of the `permissionSets` field is populated or not. Defaults to `true`. |
| `symbolStatus` | ENUM | Filters symbols that have this `tradingStatus`.   Valid values: `TRADING`, `HALT`, `BREAK`   Cannot be used in combination with `symbol` or `symbols` |

Notes:

* Only one of `symbol`, `symbols`, `permissions` parameters can be specified.
* Without parameters, `exchangeInfo` displays all symbols with `["SPOT, "MARGIN", "LEVERAGED"]` permissions.

  * In order to list *all* active symbols on the exchange, you need to explicitly request all permissions.
* `permissions` accepts either a list of permissions, or a single permission name. E.g. `"SPOT"`.
* Available Permissions

**Examples of Symbol Permissions Interpretation from the Response:**

* `[["A","B"]]` means you may place an order if your account has either permission "A" **or** permission "B".
* `[["A"],["B"]]` means you can place an order if your account has permission "A" **and** permission "B".
* `[["A"],["B","C"]]` means you can place an order if your account has permission "A" **and** permission "B" or permission "C". (Inclusive or is applied here, not exclusive or, so your account may have both permission "B" and permission "C".)

**Data Source:**
Memory

**Response:**

```
{  
  "id": "5494febb-d167-46a2-996d-70533eb4d976",  
  "status": 200,  
  "result": {  
    "timezone": "UTC",  
    "serverTime": 1655969291181,  
    // Global rate limits. See "Rate limits" section.  
    "rateLimits": [  
      {  
        "rateLimitType": "REQUEST_WEIGHT",    // Rate limit type: REQUEST_WEIGHT, ORDERS, CONNECTIONS  
        "interval": "MINUTE",                 // Rate limit interval: SECOND, MINUTE, DAY  
        "intervalNum": 1,                     // Rate limit interval multiplier (i.e., "1 minute")  
        "limit": 6000                         // Rate limit per interval  
      },  
      {  
        "rateLimitType": "ORDERS",  
        "interval": "SECOND",  
        "intervalNum": 10,  
        "limit": 50  
      },  
      {  
        "rateLimitType": "ORDERS",  
        "interval": "DAY",  
        "intervalNum": 1,  
        "limit": 160000  
      },  
      {  
        "rateLimitType": "CONNECTIONS",  
        "interval": "MINUTE",  
        "intervalNum": 5,  
        "limit": 300  
      }  
    ],  
    // Exchange filters are explained on the "Filters" page:  
    // https://github.com/binance/binance-spot-api-docs/blob/master/filters.md  
    // All exchange filters are optional.  
    "exchangeFilters": [],  
    "symbols": [  
      {  
        "symbol": "BNBBTC",  
        "status": "TRADING",  
        "baseAsset": "BNB",  
        "baseAssetPrecision": 8,  
        "quoteAsset": "BTC",  
        "quotePrecision": 8,  
        "quoteAssetPrecision": 8,  
        "baseCommissionPrecision": 8,  
        "quoteCommissionPrecision": 8,  
        "orderTypes": [  
          "LIMIT",  
          "LIMIT_MAKER",  
          "MARKET",  
          "STOP_LOSS_LIMIT",  
          "TAKE_PROFIT_LIMIT"  
        ],  
        "icebergAllowed": true,  
        "ocoAllowed": true,  
        "otoAllowed": true,  
        "quoteOrderQtyMarketAllowed": true,  
        "allowTrailingStop": true,  
        "cancelReplaceAllowed": true,  
        "amendAllowed":false,  
        "pegInstructionsAllowed": true,  
        "isSpotTradingAllowed": true,  
        "isMarginTradingAllowed": true,  
        // Symbol filters are explained on the "Filters" page:  
        // https://github.com/binance/binance-spot-api-docs/blob/master/filters.md  
        // All symbol filters are optional.  
        "filters": [  
          {  
            "filterType": "PRICE_FILTER",  
            "minPrice": "0.00000100",  
            "maxPrice": "100000.00000000",  
            "tickSize": "0.00000100"  
          },  
          {  
            "filterType": "LOT_SIZE",  
            "minQty": "0.00100000",  
            "maxQty": "100000.00000000",  
            "stepSize": "0.00100000"  
          }  
        ],  
        "permissions": [],  
        "permissionSets": [  
          [  
            "SPOT",  
            "MARGIN",  
            "TRD_GRP_004"  
          ]  
        ],  
        "defaultSelfTradePreventionMode": "NONE",  
        "allowedSelfTradePreventionModes": [  
          "NONE"  
        ]  
      }  
    ],  
    // Optional field. Present only when SOR is available.  
    // https://github.com/binance/binance-spot-api-docs/blob/master/faqs/sor_faq.md  
    "sors": [  
      {  
        "baseAsset": "BTC",  
        "symbols": [  
          "BTCUSDT",  
          "BTCUSDC"  
        ]  
      }  
    ]  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 20  
    }  
  ]  
}
```

* Test connectivity
* Check server time
* Exchange information