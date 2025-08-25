# All Coins' Information 

## API Description​

Get information of coins (available for deposit and withdraw) for user.

## HTTP Request​

GET `/sapi/v1/capital/config/getall`

## Request Weight(IP)​

10

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
[  
    {  
        "coin": "1MBABYDOGE",  
        "depositAllEnable": true,  
        "withdrawAllEnable": true,  
        "name": "1M x BABYDOGE",  
        "free": "34941.1",  
        "locked": "0",  
        "freeze": "0",  
        "withdrawing": "0",  
        "ipoing": "0",  
        "ipoable": "0",  
        "storage": "0",  
        "isLegalMoney": false,  
        "trading": true,  
        "networkList": [  
            {  
                "network": "BSC",  
                "coin": "1MBABYDOGE",  
                "withdrawIntegerMultiple": "0.01",  
                "isDefault": false,  
                "depositEnable": true,  
                "withdrawEnable": true,  
                "depositDesc": "",   // shown only when "depositEnable" is false.  
                "withdrawDesc": "",  // shown only when "withdrawEnable" is false.  
                "specialTips": "",  
                "specialWithdrawTips": "",  
                "name": "BNB Smart Chain (BEP20)",  
                "resetAddressStatus": false,  
                "addressRegex": "^(0x)[0-9A-Fa-f]{40}$",  
                "memoRegex": "",  
                "withdrawFee": "10",  
                "withdrawMin": "20",  
                "withdrawMax": "9999999999",  
                "withdrawInternalMin": "0.01", // Minimum internal transfer amount  
                "depositDust": "0.01",  
                "minConfirm": 5,  // min number for balance confirmation  
                "unLockConfirm": 0,  // confirmation number for balance unlock  
                "sameAddress": false,  // If the coin needs to provide memo to withdraw  
                "estimatedArrivalTime": 1,  
                "busy": false,  
                "contractAddressUrl": "https://bscscan.com/token/",  
                "contractAddress": "0xc748673057861a797275cd8a068abb95a902e8de",  
                "denomination": 1000000   // 1 1MBABYDOGE = 1000000 BABYDOGE  
            },  
            {  
                "network": "ETH",  
                "coin": "1MBABYDOGE",  
                "withdrawIntegerMultiple": "0.01",  
                "isDefault": true,  
                "depositEnable": true,  
                "withdrawEnable": true,  
                "depositDesc": "",  
                "withdrawDesc": "",  
                "specialTips": "",  
                "specialWithdrawTips": "",  
                "name": "Ethereum (ERC20)",  
                "resetAddressStatus": false,  
                "addressRegex": "^(0x)[0-9A-Fa-f]{40}$",  
                "memoRegex": "",  
                "withdrawFee": "1511",  
                "withdrawMin": "3022",  
                "withdrawMax": "9999999999",  
                "withdrawInternalMin": "0.01",    
                "depositDust": "0.01",  
                "minConfirm": 6,  
                "unLockConfirm": 64,  
                "sameAddress": false,    
                "estimatedArrivalTime": 2,  
                "busy": false,  
                "contractAddressUrl": "https://etherscan.io/address/",  
                "contractAddress": "0xac57de9c1a09fec648e93eb98875b212db0d460b",  
                "denomination": 1000000   
            }  
        ]  
    }  
]
```

