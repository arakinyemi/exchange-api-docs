
# Create Internal Transfer
Create the internal transfer between different account types under the same UID.


tip

Please refer to transferable coin list API to find out more.

HTTP Request
```http
POST /v5/asset/transfer/inter-transfer
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| transferId | true | string | UUID. Please manually generate a UUID |
| coin | true | string | Coin, uppercase only |
| amount | true | string | Amount |
| fromAccountType | true | string | From account type |
| toAccountType | true | string | To account type |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| transferId | string | UUID |
| status | string | Transfer status <br> STATUS_UNKNOWN <br> SUCCESS <br> PENDING <br> FAILED |

---


Request Example

HTTP
 
  
```http
POST v5/asset/transfer/inter-transfer HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1670986690556
X-BAPI-RECV-WINDOW: 50000
X-BAPI-SIGN: XXXXX
Content-Type: application/json
{
    "transferId": "42c0cfb0-6bca-c242-bc76-4e6df6cbcb16",
    "coin": "BTC",
    "amount": "0.05",
    "fromAccountType": "UNIFIED",
    "toAccountType": "CONTRACT"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "transferId": "42c0cfb0-6bca-c242-bc76-4e6df6cbab16",
        "status": "SUCCESS"
    },
    "retExtinfo
": {},
    "time": 1670986962783
}
```

