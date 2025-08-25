# Futures Transfer for Sub-account (For Master Account) 

## API Description​

Futures Transfer for Sub-account

## HTTP Request​

POST `/sapi/v1/sub-account/futures/transfer`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub-account email |
| asset | STRING | YES | The asset being transferred, e.g., USDT |
| amount | DECIMAL | YES | The amount to be transferred |
| type | INT | YES | 1: transfer from subaccount's spot account to its USDT-margined futures account 2: transfer from subaccount's USDT-margined futures account to its spot account 3: transfer from subaccount's spot account to its COIN-margined futures account 4:transfer from subaccount's COIN-margined futures account to its spot account |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to open Enable Spot & Margin Trading permission for the API Key which requests this endpoint.

## Response Example​

```
{  
    "txnId":"2966662589"  
}
```

