# Get Sub-account Deposit Address (For Master Account) 

## API Description​

Fetch sub-account deposit address

## HTTP Request​

GET `/sapi/v1/capital/deposit/subAddress`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub account email |
| coin | STRING | YES |  |
| network | STRING | NO |  |
| amount | DECIMAL | NO |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * `amount` needs to be sent if using LIGHTNING network

## Response Example​

```
{  
	"address":"TDunhSa7jkTNuKrusUTU1MUHtqXoBPKETV",  
	"coin":"USDT",  
	"tag":"",  
	"url":"https://tronscan.org/#/address/TDunhSa7jkTNuKrusUTU1MUHtqXoBPKETV"  
}
```

