# Get Transferable Coin
Query the transferable coin list between each account type


HTTP Request
```http
GET /v5/asset/transfer/query-transfer-coin-list
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| fromAccountType | true | string | From account type |
| toAccountType | true | string | To account type |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| list | array | A list of coins (as strings) |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/transfer/query-transfer-coin-list?fromAccountType=UNIFIED&toAccountType=CONTRACT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672144322595
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "list": [
            "BTC",
            "ETH"
        ]
    },
    "retExtinfo
": {},
    "time": 1672144322954
}
```
