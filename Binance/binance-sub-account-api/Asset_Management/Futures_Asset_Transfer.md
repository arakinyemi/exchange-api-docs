# Sub-account Futures Asset Transfer (For Master Account) 

## API Description​

Sub-account Futures Asset Transfer

## HTTP Request​

POST `/sapi/v1/sub-account/futures/internalTransfer`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| fromEmail | STRING | YES | Sender email |
| toEmail | STRING | YES | Recipient email |
| futuresType | LONG | YES | 1:USDT-margined Futures，2: Coin-margined Futures |
| asset | STRING | YES |  |
| amount | DECIMAL | YES |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * Master account can transfer max 2000 times a minute
> * There must be sufficient margin balance in futures wallet to execute transferring.

## Response Example​

```
{  
    "success":true,  
    "txnId":"2934662589"  
}
```

