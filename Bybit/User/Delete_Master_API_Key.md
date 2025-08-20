# Delete Master API Key
Delete the api key of master account. Use the api key pending to be delete to call the endpoint. Use master user's api key only.


tip

The API key must have one of the below permissions in order to call this endpoint..

master API key: "Account Transfer", "Subaccount Transfer", "Withdrawal"

danger

BE CAREFUL! The API key used to call this interface will be invalid immediately.


HTTP Request
```http
POST /v5/user/delete-api
```

Request Parameters
None



Response Parameters
None


Request Example

HTTP
 
  
```http
POST /v5/user/delete-api HTTP/1.1
Host: api.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1676431576621
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
    "time": 1676431577675
}
```

