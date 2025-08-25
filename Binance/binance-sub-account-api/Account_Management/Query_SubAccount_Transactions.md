# Query Sub-account Transaction Statistics (For Master Account) 

## API Description​

Query Sub-account Transaction statistics (For Master Account).

## HTTP Request​

GET `/sapi/v1/sub-account/transaction-statistics`

## Request Weight​

**60**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | YES | Sub user email |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
    "recent30BtcTotal": "0",  
    "recent30BtcFuturesTotal": "0",  
    "recent30BtcMarginTotal": "0",  
    "recent30BusdTotal": "0",  
    "recent30BusdFuturesTotal": "0",  
    "recent30BusdMarginTotal": "0",  
    "tradeInfoVos": []  
}
```

> OR

```
{  
    "recent30BtcTotal": "0",  
    "recent30BtcFuturesTotal": "0",  
    "recent30BtcMarginTotal": "0",  
    "recent30BusdTotal": "0",  
    "recent30BusdFuturesTotal": "0",  
    "recent30BusdMarginTotal": "0",  
    "tradeInfoVos": [  
        {  
            "userId": 1000138138384,  
            "btc": 0,  
            "btcFutures": 0,  
            "btcMargin": 0,  
            "busd": 0,  
            "busdFutures": 0,  
            "busdMargin": 0,  
            "date": 1676851200000  
        },  
        {  
            "userId": 1000138138384,  
            "btc": 0,  
            "btcFutures": 0,  
            "btcMargin": 0,  
            "busd": 0,  
            "busdFutures": 0,  
            "busdMargin": 0,  
            "date": 1677110400000  
        },  
        {  
            "userId": 1000138138384,  
            "btc": 0,  
            "btcFutures": 0,  
            "btcMargin": 0,  
            "busd": 0,  
            "busdFutures": 0,  
            "busdMargin": 0,  
            "date": 1677369600000  
        }  
    ]  
}
```

* API Description
* HTTP Request
* Request Weight
* Request Parameters
* Response Example