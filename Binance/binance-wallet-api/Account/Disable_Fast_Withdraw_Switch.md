# Disable Fast Withdraw Switch

## HTTP Request​

POST `/sapi/v1/account/disableFastWithdrawSwitch`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

* **Caution:**

  This request will disable fastwithdraw switch under your account.   
    
  You need to enable "trade" option for the api key which requests this endpoint.

## Response Example​

```json
{}
```

