# Fetch withdraw address list 

## API Description​

Fetch withdraw address list

## HTTP Request​

GET `/sapi/v1/capital/withdraw/address/list`

## Request Weight(IP)​

**10**

## Request Parameters​

NONE

## Response Example​

```json
[  
  {  
    "address": "1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa",  
    "addressTag": "",  
    "coin": "BTC",  
    "name": "Satoshi",        //is a user-defined name  
    "network": "BTC",  
    "origin": "bla",      //if originType != 'others', this value is blank, otherwise the origin is manually filled in by the user  
    "originType": "others",  //Address source type, including but not limited to: type Exchange Address: Binance, CoinBase, HTX, Bitfinex, OKX, Bithumb, Kraken, Kucoin, Gemini, Bitget, Bybit, Upbit, Gate.io;  type Wallet Address: Binance Web3 Wallet, Trust Wallet, MetaMask, Rabby Wallet, Phantom, OKX Web 3 Wallet, Coinbase Wallet, Bitget Wallet; type Others: others(multilanguage support)  
    "whiteStatus": true      //Is it whitelisted  
  }  
]
```

