# Create Universal Transfer
Transfer between sub-sub or main-sub.


tip

Use master or sub acct api key to request

To use sub acct api key, it must have "SubMemberTransferList" permission

When use sub acct api key, it can only transfer to main account

If you encounter errorCode: 131228 and msg: your balance is not enough, please go to # Get Single Coin Balance to check transfer safe amount.

You can not transfer between the same UID.

Currently, the funding wallet only supports out  ing transfers in cryptocurrency, not in fiat currency.

HTTP Request
```http
POST /v5/asset/transfer/universal-transfer
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| transferId | true | string | UUID. Please manually generate a UUID |
| coin | true | string | Coin, uppercase only |
| amount | true | string | Amount |
| fromMemberId | true | integer | From UID |
| toMemberId | true | integer | To UID |
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
POST /v5/asset/transfer/universal-transfer HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672189449697
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
Content-Type: application/json
```

```json
{
    "transferId": "be7a2462-1138-4e27-80b1-62653f24925e",
    "coin": "ETH",
    "amount": "0.5",
    "fromMemberId": 592334,
    "toMemberId": 691355,
    "fromAccountType": "CONTRACT",
    "toAccountType": "UNIFIED"
```

}

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "transferId": "be7a2462-1138-4e27-80b1-62653f24925e",
        "status": "SUCCESS"
    },
    "retExtinfo
": {},
    "time": 1672189450195
}
```

