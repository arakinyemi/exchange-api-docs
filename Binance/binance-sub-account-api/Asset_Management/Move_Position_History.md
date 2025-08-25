# Get Move Position History for Sub-account (For Master Account) 

## API Description​

Query move position history

## HTTP Request​

GET `/sapi/v1/sub-account/futures/move-position`

## Request Weight(IP)​

**150**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| symbol | STRING | YES |  |
| startTime | LONG | NO |  |
| endTime | LONG | NO |  |
| page | INT | YES |  |
| row | INT | YES |  |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * If `startTime` and `endTime` not sent, return records of the last 90 days by default with 1000 maximum limits
> * If `startTime` is sent and `endTime` is not sent, return records of [max(startTime, now-90d), now].
> * If `startTime` is not sent and `endTime` is sent, return records of [max(now,endTime-90d), endTime].

## Response Example​

```
{  
	"total": 3,  
	"futureMovePositionOrderVoList": [{  
		"fromUserEmail": "testFrom@google.com",  
		"toUserEmail": "testTo@google.com",  
		"productType": "UM",  
		"symbol": "BTCUSDT",  
		"price": "105025.50981609",  
		"quantity": "0.00100000",  
		"positionSide": "BOTH",  
		"side": "SELL",  
		"timeStamp": 1737544712000  
	}, {  
		"fromUserEmail": "testFrom1@google.com",  
		"toUserEmail": "testTo1@google.com",  
		"productType": "UM",  
		"symbol": "BTCUSDT",  
		"price": "97100.00000000",  
		"quantity": "0.00100000",  
		"positionSide": "BOTH",  
		"side": "SELL",  
		"timeStamp": 1740041627000  
	}, {  
		"fromUserEmail": "testFrom2@google.com",  
		"toUserEmail": "testTo2@google.com",  
		"productType": "UM",  
		"symbol": "BTCUSDT",  
		"price": "97108.62068889",  
		"quantity": "0.00100000",  
		"positionSide": "BOTH",  
		"side": "SELL",  
		"timeStamp": 1740041959000  
	}]  
}
```

