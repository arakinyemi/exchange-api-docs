### Log out of the session​

```json
{  
  "id": "c174a2b1-3f51-4580-b200-8528bd237cb7",  
  "method": "session.logout"  
}
```

Forget the API key previously authenticated.
If the connection is not authenticated, this request does nothing.

Note that the WebSocket connection stays open after `session.logout` request.
You can continue using the connection,
but now you will have to explicitly provide the `apiKey` and `signature` parameters where needed.

**Weight:**
2

**Parameters:**
NONE

**Data Source:**
Memory

**Response:**

```json
{  
  "id": "c174a2b1-3f51-4580-b200-8528bd237cb7",  
  "status": 200,  
  "result": {  
    "apiKey": null,  
    "authorizedSince": null,  
    "connectedSince": 1649729873021,  
    "returnRateLimits": false,  
    "serverTime": 1649730611671,  
    "userDataStream": false // is User Data Stream subscription active?  
  }  
}
```

