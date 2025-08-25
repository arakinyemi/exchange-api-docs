### Query session status​

```json
{  
  "id": "b50c16cd-62c9-4e29-89e4-37f10111f5bf",  
  "method": "session.status"  
}
```

Query the status of the WebSocket connection,
inspecting which API key (if any) is used to authorize requests.

**Weight:**
2

**Parameters:**
NONE

**Data Source:**
Memory

**Response:**

```json
{  
  "id": "b50c16cd-62c9-4e29-89e4-37f10111f5bf",  
  "status": 200,  
  "result": {  
    // if the connection is not authenticated, "apiKey" and "authorizedSince" will be shown as null  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "authorizedSince": 1649729878532,  
    "connectedSince": 1649729873021,  
    "returnRateLimits": false,  
    "serverTime": 1649730611671,  
    "userDataStream": true // is User Data Stream subscription active?  
  }  
}
```