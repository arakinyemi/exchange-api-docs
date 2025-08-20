# Get UID Wallet Type
Get available wallet types for the master account or sub account


tip

Master api key: you can get master account and appointed sub account available wallet types, and support up to 200 sub UID in one request.

Sub api key: you can get its own available wallet types

PRACTICE

"FUND" - If you never deposit or transfer capital into it, this wallet type will not be shown in the array, but your account indeed has this wallet.

["SPOT","FUND","CONTRACT"] : Classic account and Funding wallet was operated before

["SPOT","CONTRACT"] : Classic account and Funding wallet is never operated

["UNIFIED""FUND","CONTRACT"] : UTA account and Funding wallet was operated before.

["UNIFIED","CONTRACT"] : UTA account and Funding wallet is never operated.

HTTP Request
```http
GET /v5/user/get-member-type
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| memberIds | false | string  |Query itself wallet types when not passed <br> When use master api key to query sub UID, master UID data is always returned in the top of the array <br> Multiple sub UID are supported, separated by commas <br> This param is ignored when you use sub account api key |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| accounts | array | Object |
| > uid | string | Master/Sub user Id |
| > accountType | array | Wallets array. SPOT, CONTRACT, FUND, OPTION, UNIFIED. Please check above practice to understand the value |

---


Request Example

HTTP
 
  
```http
GET /v5/user/get-member-type HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1686884973961
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "accounts": [
            {
                "uid": "533285",
                "accountType": [
                    "SPOT",
                    "UNIFIED",
                    "FUND",
                    "CONTRACT"
                ]
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1686884974151
}
```

