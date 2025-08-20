# Set Disconnect Cancel All
info


What is Disconnection Protect (DCP)?
Based on the websocket private connection and heartbeat mechanism, Bybit provides disconnection protection function. The timing starts from the first disconnection. If the Bybit server does not receive the reconnection from the client for more than 10 (default) seconds and resumes the heartbeat "ping", then the client is in the state of "disconnection protect", all active futures / spot / option orders of the client will be cancelled automatically. If within 10 seconds, the client reconnects and resumes the heartbeat "ping", the timing will be reset and restarted at the next disconnection.

How to enable DCP

If you need to turn it on/off, you can contact your client manager for consultation and application. The default time window is 10 seconds.

Applicable

Effective for Inverse Perp / Inverse Futures / USDT Perp / USDT Futures / USDC Perp / USDC Futures / Spot / options (UTA2.0)
Effective for USDT Perp / USDT Futures / USDC Perp / USDC Futures / Spot / options (UTA1.0)


tip


After the request is successfully sent, the system needs a certain time to take effect. It is recommended to query or set again after 10 seconds

You can use this endpoint to get your current DCP configuration.
Your private websocket connection must subscribe "dcp" topic in order to trigger DCP successfully


HTTP Request
```http
POST /v5/order/disconnected-cancel-all
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| product | false | string | OPTIONS(default), DERIVATIVES, SPOT |
| timeWindow | true | integer | Disconnection timing window time. [3, 300], unit: second |

---


Response Parameters
None


Request Example

HTTP
 
  
  
```http
POST v5/order/disconnected-cancel-all HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1675852742375
X-BAPI-RECV-WINDOW: 50000
Content-Type: application/json
```

```json
{
  "timeWindow": 40
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success"
}
```

