# Enable Fast Withdraw Switch 

## API Description​

Enable Fast Withdraw Switch 

## HTTP Request​

POST `/sapi/v1/account/enableFastWithdrawSwitch`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * This request will enable fastwithdraw switch under your account.   
>     
>   You need to enable "trade" option for the api key which requests this endpoint.
> * When Fast Withdraw Switch is on, transferring funds to a Binance account will be done instantly. There is no on-chain transaction, no transaction ID and no withdrawal fee.

## Response Example​

```
{}
```

