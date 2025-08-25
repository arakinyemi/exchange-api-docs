### Start user data stream (Deprecated)​

```
POST /api/v3/userDataStream
```

Start a new user data stream. The stream will close after 60 minutes unless a keepalive is sent.

**Weight:**
2

**Parameters:**
NONE

**Data Source:**
Memory

**Response:**

```json
{  
  "listenKey": "pqia91ma19a5s61cv6a81va65sdf19v8a65a1a5s61cv6a81va65sdf19v8a65a1"  
}
```