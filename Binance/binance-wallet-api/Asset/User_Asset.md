# User Asset 

## API Description​

Get user assets, just for positive data.

## HTTP Request​

POST `/sapi/v3/asset/getUserAsset`

## Request Weight(IP)​

**5**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| asset | STRING | NO | If asset is blank, then query all positive assets user have. |
| needBtcValuation | BOOLEAN | NO | Whether need btc valuation or not. |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If asset is set, then return this asset, otherwise return all assets positive.
> * If needBtcValuation is set, then return btcValudation.

## Response Example​

```json
[  
  {  
    "asset": "AVAX",  
    "free": "1",  
    "locked": "0",  
    "freeze": "0",  
    "withdrawing": "0",  
    "ipoable": "0",  
    "btcValuation": "0"  
  },  
  {  
    "asset": "BCH",  
    "free": "0.9",  
    "locked": "0",  
    "freeze": "0",  
    "withdrawing": "0",  
    "ipoable": "0",  
    "btcValuation": "0"  
  },  
  {  
    "asset": "BNB",  
    "free": "887.47061626",  
    "locked": "0",  
    "freeze": "10.52",  
    "withdrawing": "0.1",  
    "ipoable": "0",  
    "btcValuation": "0"  
  },  
  {  
    "asset": "BUSD",  
    "free": "9999.7",  
    "locked": "0",  
    "freeze": "0",  
    "withdrawing": "0",  
    "ipoable": "0",  
    "btcValuation": "0"  
  },  
  {  
    "asset": "SHIB",  
    "free": "532.32",  
    "locked": "0",  
    "freeze": "0",  
    "withdrawing": "0",  
    "ipoable": "0",  
    "btcValuation": "0"  
  },  
  {  
    "asset": "USDT",  
    "free": "50300000001.44911105",  
    "locked": "0",  
    "freeze": "0",  
    "withdrawing": "0",  
    "ipoable": "0",  
    "btcValuation": "0"  
  },  
  {  
    "asset": "WRZ",  
    "free": "1",  
    "locked": "0",  
    "freeze": "0",  
    "withdrawing": "0",  
    "ipoable": "0",  
    "btcValuation": "0"  
  }  
]
```

