# Deposit Address(supporting network) 

## API Description​

Fetch deposit address with network.

## HTTP Request​

GET `/sapi/v1/capital/deposit/address`

## Request Weight(IP)​

**10**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| coin | STRING | YES |  |
| network | STRING | NO |  |
| amount | DECIMAL | NO |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If `network` is not send, return with default network of the coin.
> * You can get `network` and `isDefault` in `networkList` in the response of `Get /sapi/v1/capital/config/getall (HMAC SHA256)`.
> * `amount` needs to be sent if using LIGHTNING network

## Response Example​

```json
{  
	"address": "1HPn8Rx2y6nNSfagQBKy27GB99Vbzg89wv",  
 	"coin": "BTC",  
 	"tag": "",  
 	"url": "https://btc.com/1HPn8Rx2y6nNSfagQBKy27GB99Vbzg89wv"  
}
```

