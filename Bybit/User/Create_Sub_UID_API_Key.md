# Create Sub UID API Key
To create new API key for those newly created sub UID. Use master user's api key only.


tip

The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"

HTTP Request
```http
POST /v5/user/create-sub-api
```

Request Parameters
| Parameter       | Required | Type    | Comments                                                                                                                                                                                                                                                     |
|-----------------|----------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subuid          | true     | integer | Sub user Id                                                                                                                                                                                                                                                  |
| note            | false    | string  | Set a remark                                                                                                                                                                                                                                                 |
| readOnly        | true     | integer | 0：Read and Write. 1：Read only                                                                                                                                                                                                                              |
| ips             | false    | string  | Set the IP bind. example: "192.168.0.1,192.168.0.2"note:<br>don't pass ips or pass with "*" means no bind<br>No ip bound api key will be invalid after 90 days<br>api key without IP bound will be invalid after 7 days once the account password is changed |
| permissions     | true     | Object  | Tick the types of permission.<br>one of below types must be passed, otherwise the error is thrown                                                                                                                                                            |
| > ContractTrade | false    | array   | Contract Trade. ["Order","Position"]                                                                                                                                                                                                                         |
| > Spot          | false    | array   | Spot Trade. ["SpotTrade"]                                                                                                                                                                                                                                    |
| > Options       | false    | array   | USDC Contract. ["OptionsTrade"]                                                                                                                                                                                                                              |
| > Wallet        | false    | array   | Wallet. ["AccountTransfer","SubMemberTransferList"]<br>Note: Fund Custodial account is not supported                                                                                                                                                         |
| > Exchange      | false    | array   | Convert. ["ExchangeHistory"]                                                                                                                                                                                                                                 |
| > Earn          | false    | array   | Earn product. ["Earn"]                                                                                                                                                                                                                                       |
| > Derivatives   | false    | array   | This param is deprecated because system will automatically add this permission according to your account is UTA or Classic                                                                                                                                   |
| > CopyTrading   | false    | array   | Copytrade. ["CopyTrading"] deprecated because using signal copy trading now, please refer to How To Start Copy Trading                                                                                                                                       |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| id | string | Unique id. Internal used |
| note |string | The remark |
| apiKey | string | Api key |
| readOnly | integer | 0: Read and Write. 1: Read only |
| secret | string | The secret paired with api key.<br> The secret can't be queried by GET api. Please keep it properly |
| permissions | Object | The types of permission |
| > ContractTrade | array | Permisson of contract trade |
| > Spot | array | Permisson of spot |
| > Wallet | array | Permisson of wallet |
| > Options | array | Permission of USDC Contract. It supports trade option and usdc perpetual. |
| > Derivatives | array | Permission of Unified account |
| > Exchange | array | Permission of convert |
| > Earn | array | Permission of earn product |
| > CopyTrading | array | Permission of copytrade, deprecated always [] |
| > BlockTrade | array | Permission of blocktrade. Not applicable to sub account, always [] |
| > NFT | array | Deprecated, always [] |

---

Request Example

HTTP
 
  
```http
POST /v5/user/create-sub-api HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676430005459
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "subuid": 53888000,
    "note
": "testxxx",
    "readOnly": 0,
    "permissions": {
        "Wallet": [
            "AccountTransfer"
        ]
    }
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "id": "16651283",
        "note
": "testxxx",
        "apiKey": "xxxxx",
        "readOnly": 0,
        "secret": "xxxxxxxx",
        "permissions": {
            "ContractTrade": [],
            "Spot": [],
            "Wallet": [
                "AccountTransfer"
            ],
            "Options": [],
            "CopyTrading": [],
            "BlockTrade": [],
            "Exchange": [],
            "NFT": [],
            "Earn": ["Earn"]
        }
    },
    "retExtinfo
": {},
    "time": 1676430007643
}
```

