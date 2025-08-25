# Withdraw (for local entities that require travel rule) 

## API Description​

Submit a withdrawal request for local entities that required travel rule.

## HTTP Request​

POST `/sapi/v1/localentity/withdraw/apply`

## Request Weight(UID)​

**600**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| coin | STRING | YES |  |
| withdrawOrderId | STRING | NO | withdrawID defined by the client (i.e. client's internal withdrawID) |
| network | STRING | NO |  |
| address | STRING | YES |  |
| addressTag | STRING | NO | Secondary address identifier for coins like XRP,XMR etc. |
| amount | DECIMAL | YES |  |
| transactionFeeFlag | BOOLEAN | NO | When making internal transfer, `true` for returning the fee to the destination account; `false` for returning the fee back to the departure account. Default `false`. |
| name | STRING | NO | Description of the address. Address book cap is 200, space in name should be encoded into `%20` |
| walletType | INTEGER | NO | The wallet type for withdraw，0-spot wallet ，1-funding wallet. Default walletType is the current "selected wallet" under wallet->Fiat and Spot/Funding->Deposit |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |
| questionnaire | STRING | YES | JSON format questionnaire answers. |

> * If `network` not send, return with default network of the coin, but if the address could not match default network, the withdraw will be rejected.
> * You can get `network` and `isDefault` in `networkList` of a coin in the response
>   of `Get /sapi/v1/capital/config/getall (HMAC SHA256)`.
> * Questionnaire is different for each local entity, please refer to
>   the `Withdraw Questionnaire Contents` page.
> * If getting error like `Questionnaire format not valid.` or `Questionnaire must not be blank`,
>   please try to verify the format of the questionnaire and use URL-encoded format.

## Response Example​

```json
{  
  "trId": 123456, # The travel rule record Id  
  "accpted": true, # Whether the withdraw request is accepted  
  "info": "Withdraw request accepted" # The detailed infomation of the withdrawal result.  
}
```

