# Toggle BNB Burn On Spot Trade And Margin Interest 

## API Description​

Toggle BNB Burn On Spot Trade And Margin Interest

## HTTP Request​

POST `/sapi/v1/bnbBurn`

## Request Weight​

**1(IP)**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| spotBNBBurn | STRING | NO | "true" or "false"; Determines whether to use BNB to pay for trading fees on SPOT |
| interestBNBBurn | STRING | NO | "true" or "false"; Determines whether to use BNB to pay for margin loan's interest |
| recvWindow | LONG | NO | No more than 60000 |
| timestamp | LONG | YES |  |

* "spotBNBBurn" and "interestBNBBurn" should be sent at least one.

## Response Example​

```json
{  
   "spotBNBBurn":true,  
   "interestBNBBurn": false     
}
```

* API Description
* HTTP Request
* Request Weight
* Request Parameters
* Response Example