# Deposit Assets Into The Managed Sub-account (For Investor Master Account) 

## API Description​

Deposit Assets Into The Managed Sub-account

## HTTP Request​

POST `/sapi/v1/managed-subaccount/deposit`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| toEmail | STRING | YES |  |
| asset | STRING | YES |  |
| amount | DECIMAL | YES |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to enable `Enable Spot & Margin Trading` option for the api key which requests this endpoint

## Response Example​

```
{  
   "tranId":66157362489  
}
```

