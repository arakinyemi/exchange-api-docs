### Get funds transfer state
Retrieve the transfer state data of the last 2 weeks.

**Rate Limit:** 10 requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

HTTP Request
```
GET /api/v5/asset/transfer-state
```

Request Example
```
GET /api/v5/asset/transfer-state?transId=1&type=1
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| transId | String | Conditional | Transfer ID<br>Either transId or clientId is required. If both are passed, transId will be used. |
| clientId | String | Conditional | Client-supplied ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| type | String | No | Transfer type<br>0: transfer within account<br>1: master account to sub-account (Only applicable to API Key from master account)<br>2: sub-account to master account (Only applicable to API Key from master account)<br>3: sub-account to master account (Only applicable to APIKey from sub-account)<br>4: sub-account to sub-account (Only applicable to APIKey from sub-account, and target account needs to be another sub-account which belongs to same master account)<br>The default is 0.<br>For Custody accounts, can choose not to pass this parameter or pass 0. |

Response Example
```
{
    "code": "0",
    "data": [
        {
            "amt": "1.5",
            "ccy": "USDT",
            "clientId": "",
            "from": "18",
            "instId": "", //deprecated
            "state": "success",
            "subAcct": "test",
            "to": "6",
            "toInstId": "", //deprecated
            "transId": "1",
            "type": "1"
        }
    ],
    "msg": ""
}
```

Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| transId | String | Transfer ID |
| clientId | String | Client-supplied ID |
| ccy | String | Currency, e.g. USDT |
| amt | String | Amount to be transferred |
| type | String | Transfer type<br>0: transfer within account<br>1: master account to sub-account (Only applicable to API Key from master account)<br>2: sub-account to master account (Only applicable to APIKey from master account)<br>3: sub-account to master account (Only applicable to APIKey from sub-account)<br>4: sub-account to sub-account (Only applicable to APIKey from sub-account, and target account needs to be another sub-account which belongs to same master account) |
| from | String | The remitting account<br>6: Funding account<br>18: Trading account |
| to | String | The beneficiary account<br>6: Funding account<br>18: Trading account |
| subAcct | String | Name of the sub-account |
| instId | String | deprecated |
| toInstId | String | deprecated |
| state | String | Transfer state<br>success<br>pending<br>failed |
