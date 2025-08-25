# Universal Transfer (For Master Account) 

## API Description​

Universal Transfer

## HTTP Request​

POST `/sapi/v1/sub-account/universalTransfer`

## Request Weight(IP)​

**1**

## Request Weight(UID)​

**360**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| fromEmail | STRING | NO |  |
| toEmail | STRING | NO |  |
| fromAccountType | STRING | YES | "SPOT","USDT\_FUTURE","COIN\_FUTURE","MARGIN"(Cross),"ISOLATED\_MARGIN" |
| toAccountType | STRING | YES | "SPOT","USDT\_FUTURE","COIN\_FUTURE","MARGIN"(Cross),"ISOLATED\_MARGIN" |
| clientTranId | STRING | NO | Must be unique |
| symbol | STRING | NO | Only supported under ISOLATED\_MARGIN type |
| asset | STRING | YES |  |
| amount | DECIMAL | YES |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to enable "internal transfer" option for the api key which requests this endpoint.
> * Transfer from master account by default if fromEmail is not sent.
> * Transfer to master account by default if toEmail is not sent.
> * At least either fromEmail or toEmail need to be sent when the fromAccountType and the toAccountType are the same.
> * Supported transfer scenarios:
>   * `SPOT` transfer to `SPOT`, `USDT_FUTURE`, `COIN_FUTURE` (regardless of master or sub)
>   * `SPOT`, `USDT_FUTURE`, `COIN_FUTURE` transfer to `SPOT` (regardless of master or sub)
>   * Master account `SPOT` transfer to sub-account `MARGIN(Cross)`, `ISOLATED_MARGIN`
>   * Sub-account `MARGIN(Cross)`, `ISOLATED_MARGIN` transfer to master account `SPOT`
>   * Sub-account `MARGIN(Cross)` transfer to Sub-account `MARGIN(Cross)`
>   * `ALPHA` to `ALPHA` (regardless of master or sub)

## Response Example​

```
{  
    "tranId":11945860693,  
    "clientTranId":"test"  
}
```

* API Description
* HTTP Request
* Request Weight(IP)
* Request Weight(UID)
* Request Parameters
* Response Example