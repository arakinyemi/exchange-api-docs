# Get Sub-account Deposit History (For Master Account) 

## API Description​

Fetch sub-account deposit history

## HTTP Request​

GET `/sapi/v1/capital/deposit/subHisrec`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub account email |
| coin | STRING | NO |  |
| status | INT | NO | 0(0:pending,6: credited but cannot withdraw,7:Wrong Deposit,8:Waiting User confirm,1:success) |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| limit | INT | NO |  |
| offset | INT | NO | default:0 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |
| txId | STRING | NO |  |

## Response Example​

```
[  
    {  
        "id": "769800519366885376",  
        "amount": "0.001",  
        "coin": "BNB",  
        "network": "BNB",  
        "status": 0,  
        "address": "bnb136ns6lfw4zs5hg4n85vdthaad7hq5m4gtkgf23",  
        "addressTag": "101764890",  
        "txId": "98A3EA560C6B3336D348B6C83F0F95ECE4F1F5919E94BD006E5BF3BF264FACFC",  
        "insertTime": 1661493146000,  
        "transferType": 0,  
        "confirmTimes": "1/1",  
        "unlockConfirm": 0,  
        "walletType": 0  
    },  
    {  
        "id": "769754833590042625",  
        "amount":"0.50000000",  
        "coin":"IOTA",  
        "network":"IOTA",  
        "status":1,  
        "address":"SIZ9VLMHWATXKV99LH99CIGFJFUMLEHGWVZVNNZXRJJVWBPHYWPPBOSDORZ9EQSHCZAMPVAPGFYQAUUV9DROOXJLNW",  
        "addressTag":"",  
        "txId":"ESBFVQUTPIWQNJSPXFNHNYHSQNTGKRVKPRABQWTAXCDWOAKDKYWPTVG9BGXNVNKTLEJGESAVXIKIZ9999",  
        "insertTime":1599620082000,  
        "transferType":0,  
        "confirmTimes": "1/1",  
        "unlockConfirm": 0,  
        "walletType": 0  
    }  
]
```

