# Delete Sub API Key
Delete the api key of sub account. Use the sub api key pending to be delete to call the endpoint or use the master api key to delete corresponding sub account api key


tip

The API key must have one of the below permissions in order to call this endpoint.

sub API key: "Account Transfer", "Sub Member Transfer"

master API Key: "Account Transfer", "Sub Member Transfer", "Withdrawal"

danger

BE CAREFUL! The Sub account API key will be invalid immediately after calling the endpoint.


HTTP Request
```http
POST /v5/user/delete-sub-api
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| apikey | false | string | Sub account api key <br> You must pass this param when you use master account manage sub account api key settings <br> If you use corresponding sub uid api key call this endpoint, apikey param cannot be passed, otherwise throwing an error |

---


Response Parameters
None


Request Example

HTTP
 
  
```http
POST /v5/user/delete-sub-api HTTP/1.1
Host: api.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676431922953
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
Content-Type: application/json
```

```json
{

}
```



Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {},
    "retExtinfo
": {},
    "time": 1676431924719
}
```

