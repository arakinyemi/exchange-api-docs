### Login
Request Example

```json
{
  "op": "login",
  "args": [
    {
      "apiKey": "******",
      "passphrase": "******",
      "timestamp": "1538054050",
      "sign": "7L+zFQ+CEgGu5rzCj4+BdV2/uUHGqddA9pI6ztsRRPs="
    }
  ]
}
Request Parameters
| Parameter | Type              | Required | Description                                                                 |
|-----------|-------------------|----------|-----------------------------------------------------------------------------|
| op        | String            | Yes      | Operation type; for login, use `"login"`.                                   |
| args      | Array of objects  | Yes      | List of accounts to log in.                                                  |
| > apiKey  | String            | Yes      | Your API Key.                                                               |
| > passphrase | String         | Yes      | The passphrase you set when creating the API key.                            |
| > timestamp | String          | Yes      | Unix Epoch time in seconds.                                                  |
| > sign    | String            | Yes      | Signature string generated using HMAC SHA256 with your SecretKey.             |
```

Successful Response Example

```json
{
  "event": "login",
  "code": "0",
  "msg": "",
  "connId": "a4d3ae55"
}
Failure Response Example
```

```json
{
  "event": "error",
  "code": "60009",
  "msg": "Login failed.",
  "connId": "a4d3ae55"
}
```

Response parameters
| Parameter | Type   | Required | Description                                                                 |
|-----------|--------|----------|-----------------------------------------------------------------------------|
| event     | String | Yes      | Operation type; e.g., "login".                                             |
| error     |        | No       | Contains error information if login fails.                                  |
| code      | String | No       | Error code, if any.                                                         |
| msg       | String | No       | Error message, if any.                                                      |
| connId    | String | Yes      | WebSocket connection ID.                                                    |
| apiKey    |        | Yes      | Unique identification for invoking API. Requires user to apply one manually.|

passphrase: API Key password

timestamp: the Unix Epoch time, the unit is seconds, e.g. 1704876947

sign: signature string, the signature algorithm is as follows:

First concatenate timestamp, method, requestPath, strings, then use HMAC SHA256 method to encrypt the concatenated string with SecretKey, and then perform Base64 encoding.

secretKey: The security key generated when the user applies for API key, e.g. 22582BD0CFF14C41EDBF1AB98506286D

Example of timestamp: const timestamp = '' + Date.now() / 1,000

Among sign example: sign=CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(timestamp +'GET'+'/users/self/verify', secretKey))

method: always 'GET'.

requestPath : always '/users/self/verify'

 The request will expire 30 seconds after the timestamp. If your server time differs from the API server time, we recommended using the REST API to query the API server time and then set the timestamp.
