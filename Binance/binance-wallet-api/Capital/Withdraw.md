# Withdraw

## API Description​

Submit a withdraw request.

## HTTP Request​

POST `/sapi/v1/capital/withdraw/apply`

## Request Weight(UID)​

**900**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| coin | STRING | YES |  |
| withdrawOrderId | STRING | NO | client side id for withdrawal, if provide here, can be used in GET `/sapi/v1/capital/withdraw/history` for query. |
| network | STRING | NO |  |
| address | STRING | YES |  |
| addressTag | STRING | NO | Secondary address identifier for coins like XRP,XMR etc. |
| amount | DECIMAL | YES |  |
| transactionFeeFlag | BOOLEAN | NO | When making internal transfer, `true` for returning the fee to the destination account; `false` for returning the fee back to the departure account. Default `false`. |
| name | STRING | NO | Description of the address. Address book cap is 200, space in name should be encoded into `%20` |
| walletType | INTEGER | NO | The wallet type for withdraw，0-spot wallet ，1-funding wallet. Default walletType is the current "selected wallet" under wallet->Fiat and Spot/Funding->Deposit |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If `network` not send, return with default network of the coin.
> * You can get `network` and `isDefault` in `networkList` of a coin in the response of `Get /sapi/v1/capital/config/getall (HMAC SHA256)`.
> * To check if travel rule is required, by using `GET /sapi/v1/localentity/questionnaire-requirements` and if it returns anything other than `NIL` you will need update SAPI to `POST /sapi/v1/localentity/withdraw/apply` else you can continue `POST /sapi/v1/capital/withdraw/apply`. Please note that if you are required to comply to travel rule please refer to the Travel Rule SAPI.

## Response Example​

```json
{  
    "id":"7213fea8e94b4a5593d507237e5a555b"  
}
```

