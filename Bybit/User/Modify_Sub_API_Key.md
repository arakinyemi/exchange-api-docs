# Modify Sub API Key
Modify the settings of sub api key. Use the sub account api key pending to be modified to call the endpoint or use master account api key to manage its sub account api key.


tip

The API key must have one of the below permissions in order to call this endpoint

sub API key: "Account Transfer", "Sub Member Transfer"
master API Key: "Account Transfer", "Sub Member Transfer", "Withdrawal"

HTTP Request
```http
POST /v5/user/update-sub-api
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| apikey | false | string | Sub account api key <br> You must pass this param when you use master account manage sub account api key settings <br> If you use corresponding sub uid api key call this endpoint, apikey param cannot be passed, otherwise throwing an error |
| readOnly | false | integer | 0 (default)：Read and Write. 1：Read only |
| ips | false | string | Set the IP bind. example: "192.168.0.1,192.168.0.2"note: <br> don't pass ips or pass with "*" means no bind<br> No ip bound api key will be invalid after 90 days <br> api key will be invalid after 7 days once the account password is changed |
| permissions | false | Object | Tick the types of permission. Don't send this param if you don't want to change the permission |
| > ContractTrade | false | array | Contract Trade. ["Order","Position"] |
| > Spot | false | array | Spot Trade. ["SpotTrade"] |
| > Wallet | false | array | Wallet. ["AccountTransfer", "SubMemberTransferList"] <br> Note: fund custodial account is not supported |
| > Options | false | array | USDC Contract. ["OptionsTrade"] |
| > Derivatives | false | array | This param is deprecated because system will automatically add this permission according to your account is UTA or Classic |
| > Exchange | false | array | Convert. ["ExchangeHistory"] |
| > Earn | false | array | Earn product. ["Earn"] |
| > CopyTrading | false | array | Copytrade. ["CopyTrading"] |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| id | string | Unique id. Internal used |
| note | string | The remark |
| apiKey | string | Api key |
| readOnly | integer | 0：Read and Write. 1：Read only |
| secret | string | Always "" |
| permissions | Object | The types of permission |
| > ContractTrade | array | Permisson of contract trade |
| > Spot | array | Permisson of spot |
| > Wallet | array | Permisson of wallet |
| > Options | array | Permission of USDC Contract. It supports trade option and usdc perpetual. |
| > Derivatives | array | Permission of Unified account |
| > CopyTrading | array | Permission of copytrade |
| > BlockTrade | array | Permission of blocktrade. Not applicable to sub account, always [] |
| > Exchange | array | Permission of convert |
| > Earn | array | Permission of Earn |
| > NFT | array | Deprecated, always [] |
| ips | array | IP bound |

---

Request Example

HTTP
 
  
```http
POST /v5/user/update-sub-api HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676431795752
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
```

```json
{
    "readOnly": 0,
    "ips": "*",
    "permissions": {
            "ContractTrade": [],
            "Spot": [
                "SpotTrade"
            ],
            "Wallet": [
                "AccountTransfer"
            ],
            "Options": [],
            "CopyTrading": [],
            "BlockTrade": [],
            "Exchange": [],
            "NFT": []
        }
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "id": "16651472",
        "note
": "testxxx",
        "apiKey": "xxxxxx",
        "readOnly": 0,
        "secret": "",
        "permissions": {
            "ContractTrade": [],
            "Spot": [
                "SpotTrade"
            ],
            "Wallet": [
                "AccountTransfer"
            ],
            "Options": [],
            "Derivatives": [],
            "CopyTrading": [],
            "BlockTrade": [],
            "Exchange": [],
            "NFT": []
        },
        "ips": [
            "*"
        ]
    },
    "retExtinfo
": {},
    "time": 1676431796263
}
```

