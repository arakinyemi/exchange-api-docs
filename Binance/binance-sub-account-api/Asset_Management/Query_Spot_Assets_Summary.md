# Query Sub-account Spot Assets Summary (For Master Account) 

## API Description​

Get BTC valued asset summary of subaccounts.

## HTTP Request​

GET `/sapi/v1/sub-account/spotSummary`

## Request Weight(IP)​

**1**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| email | STRING | NO | Sub account email |
| page | LONG | NO | default 1 |
| size | LONG | NO | default 10, max 20 |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

## Response Example​

```
{  
	"totalCount":2,  
	"masterAccountTotalAsset": "0.23231201",  
	"spotSubUserAssetBtcVoList":[  
		{  
			"email":"sub123@test.com",  
			"totalAsset":"9999.00000000"  
		},  
		{  
			"email":"test456@test.com",  
			"totalAsset":"0.00000000"  
		}  
	]  
}
```

