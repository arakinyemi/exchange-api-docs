# Get Open Symbol List 

## API Description​

Get the list of symbols that are scheduled to be opened for trading in the market.

## HTTP Request​

GET `/sapi/v1/spot/open-symbol-list`

## Request Weight(IP)​

**100**

## Request Parameters​

No parameters required.

## Response Example​

```json
[  
  {  
    "openTime": 1686161202000,  
    "symbols": [  
      "BNBBTC",  
      "BNBETH"  
    ]  
  },  
  {  
    "openTime": 1686222232000,  
    "symbols": [  
      "BTCUSDT"  
    ]  
  }  
]
```

