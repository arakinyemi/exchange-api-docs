### WS / Status channel
Get the status of system maintenance and receive push notifications when rescheduling or when system maintenance status and end time changes.

- **First subscription:** Push the latest change data.
- **Subsequent pushes:** Every time there is a state change, push the changed content.
- **Note:** Planned maintenance causing short interruptions (less than 5 seconds) or websocket disconnections (users can immediately reconnect) will not be announced.
- Maintenance only occurs during times of low market volatility.

---

URL Path

`/ws/v5/public`

---

Request Example

```json
{
  "id": "unique-request-id",
  "op": "subscribe",
  "args": [
    {
      "channel": "status"
    }
  ]
}
```

---

Request Parameters

| Parameter | Type   | Required | Description                                                                                     |
|-----------|--------|----------|-------------------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message. Provided by client. Returned in response for tracking.          |
| op        | String | Yes      | `subscribe` or `unsubscribe`                                                                  |
| args      | Array  | Yes      | List of subscribed channels                                                                     |
| > channel | String | Yes      | Channel name, here: `status`                                                                    |

---


Response Parameters

| Parameter | Type   | Description                                  |
|-----------|--------|----------------------------------------------|
| id        | String | Unique identifier of the message             |
| event     | String | `subscribe` / `unsubscribe` / `error`       |
| arg       | Object | Subscribed channel                            |
| > channel | String | Channel name: `status`                        |
| code      | String | Error code (if any)                           |
| msg       | String | Error message (if any)                        |
| connId    | String | WebSocket connection ID                       |

---

Push Data Example

Push data parameters

| Parameter      | Type     | Description                                                                                              |
|----------------|----------|----------------------------------------------------------------------------------------------------------|
| arg            | Object   | Successfully subscribed channel                                                                          |
| > channel      | String   | Channel name                                                                                             |
| data           | Array    | Array of objects with subscribed data                                                                   |

Subscribed data object fields

| Parameter      | Type   | Description                                                                                              |
|----------------|--------|----------------------------------------------------------------------------------------------------------|
| title          | String | Title of system maintenance instructions                                                                |
| state          | String | System maintenance status: `scheduled` (waiting) `ongoing` (processing) `pre_open` (pre-open) `completed` (finished) `canceled` (canceled) Note: `pre_open` generally lasts about 10 minutes and occurs when upgrade duration is long. |
| begin          | String | Start time of system maintenance, Unix timestamp in milliseconds, e.g. `1617788463867`                   |
| end            | String | Resume trading time. Expected end time before `completed`; actual end time after `completed`. Unix timestamp in milliseconds. |
| preOpenBegin   | String | Time of `pre_open` phase. Client actions such as canceling orders, placing Post Only orders, and transferring funds to trading accounts resume at this point. |
| href           | String | Hyperlink for system maintenance details. Empty if none.                                                |
| serviceType    | String | Service type:`0`: WebSocket `5`: Trading service `6`: Block trading `7`: Trading bot `8`: Trading service (batch accounts) `9`: Trading service (batch products) `10`: Spread trading `11`: Copy trading `99`: Others (e.g. suspend partial instruments) |
| system         | String | System affected, e.g. `unified` (Trading account)                                                       |
| scheDesc       | String | Rescheduled description, e.g. `Rescheduled from 2021-01-26T16:30:00.000Z to 2021-01-28T16:30:00.000Z`    |
| maintType      | String | Maintenance type: `1`: Scheduled maintenance `2`: Unscheduled maintenance `3`: System disruption |
| env            | String | Environment: `1`: Production Trading `2`: Demo Trading                                         |
| ts             | String | Push time due to change event, Unix timestamp in milliseconds, e.g. `1617788463867`                     |

---
