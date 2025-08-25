# Get Managed Sub-account Deposit Address (For Investor Master Account) 

## API Description​

Get investor's managed sub-account deposit address.

## HTTP Request​

`GET /sapi/v1/managed-subaccount/deposit/address`

## Request Weight(UID)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub user email |
| coin | STRING | YES |  |
| network | STRING | NO | networks can be found in `GET /sapi/v1/capital/deposit/address` |
| amount | DECIMAL | NO |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If `network` is not send, return with default `network` of the `coin`.
> * * `amount` needs to be sent if using LIGHTNING network

## Response Example​

```
{  
    "coin": "USDT",  
    "address": "0x206c22d833bb0bb2102da6b7c7d4c3eb14bcf73d",  
    "tag": "",  
    "url": "https://etherscan.io/address/0x206c22d833bb0bb2102da6b7c7d4c3eb14bcf73d"  
}
```

