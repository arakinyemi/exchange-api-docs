# System Status

Listen to system status for platform maintenance or service incident.

> **info**  
> Short interruptions (<10 seconds) or websocket disconnections not announced.

### URL

- Mainnet:  
  `wss://stream.bybit.com/v5/public/misc/status`  
- EU users (from www.bybit.eu):  
  `wss://stream.bybit.eu/v5/public/misc/status`

### Topic

- `system.status`

### Response Parameters

| Parameter     | Type    | Comments                              |
|---------------|---------|-------------------------------------|
| topic         | string  | Topic name                          |
| ts            | number  | Timestamp (ms) system generates data |
| data          | array   | Object                             |

Inside `data` array objects:

| Parameter      | Type     | Comments                                                            |
|----------------|----------|---------------------------------------------------------------------|
| id             | string   | Unique identifier                                                   |
| title          | string   | System maintenance title                                            |
| state          | string   | System state                                                       |
| begin          | string   | Start time of system maintenance (ms timestamp)                    |
| end            | string   | End time or expected end time (ms timestamp)                       |
| href           | string   | Hyperlink to maintenance details, default empty                    |
| serviceTypes   | array`<int>` | Service Type                                                       |
| product        | array`<int>` | Product                                                          |
| uidSuffix      | array`<int>`; | Affected UID tail numbers                                          |
| maintainType   | string   | Maintenance type                                                   |
| env            | string   | Environment                                                       |

### Subscribe Example

```json
{
    "op": "subscribe",
    "args": [
        "system.status"
    ]
}
```

### Response Example

```json
{
    "topic": "system.status",
    "ts": 1751858399649,
    "data": [
        {
            "id": "4d95b2a0-587f-11f0-bcc9-56f28c94d6ea",
            "title": "t06",
            "state": "completed",
            "begin": "1751596902000",
            "end": "1751597011000",
            "href": "",
            "serviceTypes": [
                2,
                3,
                4,
                5
            ],
            "product": [
                1,
                2
            ],
            "uidSuffix": [],
            "maintainType": 1,
            "env": 1
        }
    ]
}
```

