# Query User Delegation History(For Master Account)

## API Description​

Query User Delegation History

## HTTP Request​

GET `/sapi/v1/asset/custody/transfer-history`

## Request Weight(IP)​

**60**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES |  |
| startTime | LONG | YES |  |
| endTime | LONG | YES |  |
| type | ENUM | NO | Delegate/Undelegate |
| asset | STRING | NO |  |
| current | INTEGER | NO | default 1 |
| size | INTEGER | NO | default 10, max 100 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to open Enable Spot & Margin Trading permission for the API Key which requests this endpoint

## Response Example​

```json
{  
    "total": 3316,  
    "rows": [  
      {  
        "clientTranId": "293915932290879488",  
        "transferType": "Undelegate",  
        "asset": "ETH",  
        "amount": "1",  
        "time": 1695205406000  
      },   
      {  
        "clientTranId": "293915892281413632",  
        "transferType": "Delegate",  
        "asset": "ETH",  
        "amount": "1",  
        "time": 1695205396000  
      }  
    ]  
}
```

