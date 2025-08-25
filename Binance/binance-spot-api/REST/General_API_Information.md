* The following base endpoints are available. Please use whichever works best for your setup:
  * **<https://api.binance.com>**
  * **<https://api-gcp.binance.com>**
  * **<https://api1.binance.com>**
  * **<https://api2.binance.com>**
  * **<https://api3.binance.com>**
  * **<https://api4.binance.com>**
* The last 4 endpoints in the point above (`api1`-`api4`) should give better performance but have less stability.
* Responses are in JSON by default. To receive responses in SBE, refer to the SBE FAQ page.
* Data is returned in **chronological order**, unless noted otherwise.
  * Without `startTime` or `endTime`, returns the most recent items up to the limit.
  * With `startTime`, returns oldest items from `startTime` up to the limit.
  * With `endTime`, returns most recent items up to `endTime` and the limit.
  * With both, behaves like `startTime` but does not exceed `endTime`.
* All time and timestamp related fields in the JSON responses are in **milliseconds by default.** To receive the information in microseconds, please add the header `X-MBX-TIME-UNIT:MICROSECOND` or `X-MBX-TIME-UNIT:microsecond`.
* We support HMAC, RSA, and Ed25519 keys. For more information, please see API Key types.
* Timestamp parameters (e.g. `startTime`, `endTime`, `timestamp`) can be passed in milliseconds or microseconds.
* For APIs that only send public market data, please use the base endpoint **<https://data-api.binance.vision>**. Please refer to Market Data Only page.
* If there are enums or terms you want clarification on, please see the SPOT Glossary for more information.
* APIs have a timeout of 10 seconds when processing a request. If a response from the Matching Engine takes longer than this, the API responds with "Timeout waiting for response from backend server. Send status unknown; execution status unknown." (-1007 TIMEOUT)
  * This does not always mean that the request failed in the Matching Engine.
  * If the status of the request has not appeared in User Data Stream, please perform an API query for its status.


* For `GET` endpoints, parameters must be sent as a `query string`.
* For `POST`, `PUT`, and `DELETE` endpoints, the parameters may be sent as a
  `query string` or in the `request body` with content type
  `application/x-www-form-urlencoded`. You may mix parameters between both the
  `query string` and `request body` if you wish to do so.
* Parameters may be sent in any order.
* If a parameter sent in both the `query string` and `request body`, the
  `query string` parameter will be used.