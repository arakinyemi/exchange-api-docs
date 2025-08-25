### Listen Key Management (Deprecated)​

> [!IMPORTANT]
> These requests have been deprecated, which means we will remove them in the future.
> Please subscribe to the User Data Stream through the WebSocket API instead.

The following requests manage User Data Stream subscriptions.

#### Start user data stream (Deprecated)​

```json
{  
  "id": "d3df8a61-98ea-4fe0-8f4e-0fcea5d418b0",  
  "method": "userDataStream.start",  
  "params": {  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A"  
  }  
}
```

Start a new user data stream.

**Note:** the stream will close in 60 minutes
unless `userDataStream.ping` requests are sent regularly.

**Weight:**
2

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `apiKey` | STRING | YES |  |

**Data Source:**
Memory

**Response:**

Subscribe to the received listen key on WebSocket Stream afterwards.

```json
{  
  "id": "d3df8a61-98ea-4fe0-8f4e-0fcea5d418b0",  
  "status": 200,  
  "result": {  
    "listenKey": "xs0mRXdAKlIPDRFrlPcw0qI41Eh3ixNntmymGyhrhgqo7L6FuLaWArTD7RLP"  
  },  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 2  
    }  
  ]  
}
```

#### Ping user data stream (Deprecated)​

```json
{  
  "id": "815d5fce-0880-4287-a567-80badf004c74",  
  "method": "userDataStream.ping",  
  "params": {  
    "listenKey": "xs0mRXdAKlIPDRFrlPcw0qI41Eh3ixNntmymGyhrhgqo7L6FuLaWArTD7RLP",  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A"  
  }  
}
```

Ping a user data stream to keep it alive.

User data streams close automatically after 60 minutes,
even if you're listening to them on WebSocket Streams.
In order to keep the stream open, you have to regularly send pings using the `userDataStream.ping` request.

It is recommended to send a ping once every 30 minutes.

**Weight:**
2

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `listenKey` | STRING | YES |  |
| `apiKey` | STRING | YES |  |

**Data Source:**
Memory

**Response:**

```json
{  
  "id": "815d5fce-0880-4287-a567-80badf004c74",  
  "status": 200,  
  "response": {},  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 2  
    }  
  ]  
}
```

#### Stop user data stream (USER\_STREAM) (Deprecated)​

```json
{  
  "id": "819e1b1b-8c06-485b-a13e-131326c69599",  
  "method": "userDataStream.stop",  
  "params": {  
    "listenKey": "xs0mRXdAKlIPDRFrlPcw0qI41Eh3ixNntmymGyhrhgqo7L6FuLaWArTD7RLP",  
    "apiKey": "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A"  
  }  
}
```

Explicitly stop and close the user data stream.

**Weight:**
2

**Parameters:**

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `listenKey` | STRING | YES |  |
| `apiKey` | STRING | YES |  |

**Data Source:**
Memory

**Response:**

```json
{  
  "id": "819e1b1b-8c06-485b-a13e-131326c69599",  
  "status": 200,  
  "response": {},  
  "rateLimits": [  
    {  
      "rateLimitType": "REQUEST_WEIGHT",  
      "interval": "MINUTE",  
      "intervalNum": 1,  
      "limit": 6000,  
      "count": 2  
    }  
  ]  
}
```

