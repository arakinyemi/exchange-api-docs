# Create a Virtual Sub-account (For Master Account) 

## API Description​

Create a Virtual Sub-account

## HTTP Request​

POST `/sapi/v1/sub-account/virtualSubAccount`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| subAccountString | STRING | YES | Please input a string. We will create a virtual email using that string for you to register |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * This request will generate a virtual sub account under your master account.
> * You need to enable "trade" option for the API Key which requests this endpoint.

## Response Example​

```json
{  
    "email":"addsdd_virtual@aasaixwqnoemail.com"  
}
```

