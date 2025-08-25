# Account API Trading Status 

## API Description​

Fetch account api trading status detail.

## HTTP Request​

GET `/sapi/v1/account/apiTradingStatus`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```json
{  
	"data": {          // API trading status detail  
			"isLocked": false,   // API trading function is locked or not  
			"plannedRecoverTime": 0,  // If API trading function is locked, this is the planned recover time  
			"triggerCondition": {   
					"GCR": 150,  // Number of GTC orders  
					"IFER": 150, // Number of FOK/IOC orders  
					"UFR": 300   // Number of orders  
			},  
			"updateTime": 1547630471725     
	}  
}
```

