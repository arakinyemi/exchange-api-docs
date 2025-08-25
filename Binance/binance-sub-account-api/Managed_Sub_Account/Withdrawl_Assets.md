# Withdrawl Assets From The Managed Sub-account (For Investor Master Account) 

## API Description​

Withdrawl Assets From The Managed Sub-account

## HTTP Request​

`POST /sapi/v1/managed-subaccount/withdraw`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| fromEmail | STRING | YES |  |
| asset | STRING | YES |  |
| amount | DECIMAL | YES |  |
| transferDate | LONG | NO | Withdrawals is automatically occur on the transfer date(UTC0). If a date is not selected, the withdrawal occurs right now |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to enable `Enable Spot & Margin Trading` option for the api key which requests this endpoint

## Response Example​

```
{  
    "tranId":66157362489  
}
```

