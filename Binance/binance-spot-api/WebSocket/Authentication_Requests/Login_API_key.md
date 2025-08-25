### Log in with API key (SIGNED)​

```
{  
  "id": "c174a2b1-3f51-4580-b200-8528bd237cb7",  
  "method": "session.logon",  
  "params": {  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature": "1cf54395b336b0a9727ef27d5d98987962bc47aca6e13fe978612d0adee066ed",  
    "timestamp": 1649729878532  
  }  
}
```

Authenticate WebSocket connection using the provided API key.

After calling `session.logon`, you can omit `apiKey` and `signature` parameters for future requests that require them.

Note that only one API key can be authenticated.
Calling `session.logon` multiple times changes the current authenticated API key.

**Weight:**
2

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `apiKey` | STRING | YES |  |
| `recvWindow` | LONG | NO | The value cannot be greater than `60000` |
| `signature` | STRING | YES |  |
| `timestamp` | LONG | YES |  |

**Data Source:**
Memory

**Response:**

```
{  
  "id": "c174a2b1-3f51-4580-b200-8528bd237cb7",  
  "status": 200,  
  "result": {  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "authorizedSince": 1649729878532,  
    "connectedSince": 1649729873021,  
    "returnRateLimits": false,  
    "serverTime": 1649729878630,  
    "userDataStream": false // is User Data Stream subscription active?  
  }  
}
```