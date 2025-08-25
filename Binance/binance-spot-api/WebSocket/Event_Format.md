## Event Format

User Data Stream events for non-SBE sessions are sent as JSON in **text frames**, one event per frame.

Events in SBE sessions will be sent as **binary frames**.

Please refer to [`userDataStream.subscribe`](/docs/binance-spot-api-docs/websocket-api/user-data-stream-requests#user_data_stream_subscribe) for details on how to subscribe to User Data Stream in WebSocket API.

Example of an event:

```
{  
  "event": {  
    "e": "outboundAccountPosition",  
    "E": 1728972148778,  
    "u": 1728972148778,  
    "B": [  
      {  
        "a": "ABC",  
        "f": "11818.00000000",  
        "l": "182.00000000"  
      },  
      {  
        "a": "DEF",  
        "f": "10580.00000000",  
        "l": "70.00000000"  
      }  
    ]  
  }  
}
```

Event fields:

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| `event` | OBJECT | YES | Event payload. See User Data Streams |