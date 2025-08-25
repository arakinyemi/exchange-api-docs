# Withdraw History (for local entities that require travel rule) (supporting network) 

## API Description​

Fetch withdraw history for local entities that required travel rule.

## HTTP Request​

GET `/sapi/v1/localentity/withdraw/history`

## Request Weight(IP)​

**18000**
Request limit: 10 requests per second

> * This endpoint specifically uses per second IP rate limit, user's total second level IP rate
>   limit is 180000/second. Response from the endpoint contains header
>   key `X-SAPI-USED-IP-WEIGHT-1S`, which defines weight used by the current IP.

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| trId | STRING | NO | Comma(,) separated list of travel rule record Ids. |
| txId | STRING | NO | Comma(,) separated list of transaction Ids. |
| withdrawOrderId | STRING | NO | Comma(,) separated list of withdrawID defined by the client (i.e. client's internal withdrawID). |
| network | STRING | NO |  |
| coin | STRING | NO |  |
| travelRuleStatus | INTEGER | NO | 0:Completed,1:Pending,2:Failed |
| offset | INT | NO | Default: 0 |
| limit | INT | NO | Default: 1000, Max: 1000 |
| startTime | LONG | NO | Default: 90 days from current timestamp |
| endTime | LONG | NO | Default: present timestamp |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * `network` may not be in the response for old withdraw.
> * Please notice the default `startTime` and `endTime` to make sure that time interval is within
>   0-90 days.
> * If both `startTime` and `endTime`are sent, time between `startTime`and `endTime`must be less
>   than 90 days.

## Response Example​

```json
[  
  {  
    "id": "b6ae22b3aa844210a7041aee7589627c",  // Withdrawal id in Binance  
    "trId": 1234456,  // Travel rule record id  
    "amount": "8.91000000",   // withdrawal amount  
    "transactionFee": "0.004", // only available for sAPI requests  
    "coin": "USDT",  
    "withdrawalStatus": 6, // Capital withdrawal status, only available for sAPI requests  
    "travelRuleStatus": 0, // Travel rule status.  
    "address": "0x94df8b352de7f46f64b01d3666bf6e936e44ce60",  
    "addressTag": "1231212",   
    "txId": "0xb5ef8c13b968a406cc62a93a8bd80f9e9a906ef1b3fcf20a2e48573c17659268"   // withdrawal transaction id  
    "applyTime": "2019-10-12 11:12:02",  // UTC time  
    "network": "ETH",  
    "transferType": 0 // 1 for internal transfer, 0 for external transfer, only available for sAPI requests    
    "withdrawOrderId": "WITHDRAWtest123", // will not be returned if there's no withdrawOrderId for this withdraw, only available for sAPI requests  
    "info": "The address is not valid. Please confirm with the recipient",  // reason for withdrawal failure, only available for sAPI requests  
    "confirmNo":3,  // confirm times for withdraw, only available for sAPI requests  
    "walletType": 1,  //1: Funding Wallet 0:Spot Wallet, only available for sAPI requests  
    "txKey": "", // only available for sAPI requests  
    "questionnaire": "{\'question1\':\'answer1\',\'question2\':\'answer2\'}", // The answers of the questionnaire  
    "completeTime": "2023-03-23 16:52:41" // complete UTC time when user's asset is deduct from withdrawing, only if status =  6(success)  
  },  
  {  
    "id": "156ec387f49b41df8724fa744fa82719",  
    "trId": 2231556234,  
    "amount": "0.00150000",  
    "transactionFee": "0.004",  
    "coin": "BTC",  
    "withdrawalStatus": 6,   
    "travelRuleStatus": 0,   
    "address": "1FZdVHtiBqMrWdjPyRPULCUceZPJ2WLCsB",  
    "txId": "60fd9007ebfddc753455f95fafa808c4302c836e4d1eebc5a132c36c1d8ac354"  
    "applyTime": "2019-09-24 12:43:45",  
    "network": "BTC",  
    "transferType": 0,   
    "info": "",  
    "confirmNo": 2,  
    "walletType": 1,  
    "txKey": "",  
    "questionnaire": "{\'question1\':\'answer1\',\'question2\':\'answer2\'}",  
    "completeTime": "2023-03-23 16:52:41"   
  }  
]
```

