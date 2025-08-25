# Move Position for Sub-account (For Master Account) 

## API Description​

Move position between sub-master, master-sub, or sub-sub accounts when necessary

## HTTP Request​

POST `/sapi/v1/sub-account/futures/move-position`

## Request Weight(ORDER)​

**150**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| fromUserEmail | STRING | YES |  |
| toUserEmail | STRING | YES |  |
| productType | STRING | YES | Only support UM |
| orderArgs | LIST<JSON> | YES | Max 10 positions supported. When input request parameter,orderArgs.symbol should be STRING, orderArgs.quantity should be BIGDECIMAL, and orderArgs.positionSide should be STRING, positionSide support BOTH,LONG and SHORT. Each entry should be like orderArgs[0].symbol=BTCUSDT,orderArgs[0].quantity=0.001,orderArgs[0].positionSide=BOTH. Example of the request parameter array: orderArgs[0].symbol=BTCUSDT orderArgs[0].quantity=0.001 orderArgs[0].positionSide=BOTH orderArgs[1].symbol=ETHUSDT orderArgs[1].quantity=0.01 orderArgs[1].positionSide=BOTH |
| recvWindow | LONG | NO |  |
| timestamp | LONG | YES |  |

> * You need to Enable Trading permission for the API Key which requests this endpoint.
> * This function only support VIP level 7-9.
> * Only master account can use the function
> * Quantity should be positive number only
> * The function support normal account, PM PRO and PM PRO SPAN.
> * Only support for from account has positions
> * For all orders in the same orderArgs request, if any symbol’s total close position quantity is bigger than the symbol’s current position quantity, all batch orders in the same list will fail simultaneously.
> * Only support cross margin mode
> * The price for move position is MarkPrice only.
> * Not support for MSA.
> * Not support for the symbol under Reduce-Only.

## Response Example​

```
{  
	"movePositionOrders": [{  
		"fromUserEmail": "testFrom@google.com",  
		"toUserEmail": "testTo@google.com",  
		"productType": "UM",  
		"symbol": "BTCUSDT",  
		"priceType": "MARK_PRICE",  
		"price": "97139.00000000",  
		"quantity": "0.001",  
		"positionSide": "BOTH",  
		"side": "BUY",  
		"success": true  
	}, {  
		"fromUserEmail": "testFrom1@google.com",  
		"toUserEmail": "1testTo@google.com",  
		"productType": "UM",  
		"symbol": "BTCUSDT",  
		"priceType": "MARK_PRICE",  
		"price": "97139.00000000",  
		"quantity": "0.0011",  
		"positionSide": "BOTH",  
		"side": "BUY",  
		"success": true  
	}]  
}
```

* API Description
* HTTP Request
* Request Weight(ORDER)
* Request Parameters
* Response Example