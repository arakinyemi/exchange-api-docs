# Get Bybit Server Time

HTTP Request

GET /v5/market/time

Request Parameters

None


Response Parameters
| Parameter  | Type   | Comments                      |
|------------|--------|-------------------------------|
| timeSecond | string | Bybit server timestamp (sec)  |
| timeNano   | string | Bybit server timestamp (nano) |

Request Example

HTTP
````http 
GET /v5/market/time HTTP/1.1 
````
Host: api.bybit.com

Response Example
````json
{
    "retCode": 0,
    "retMsg": "OK",
    "result": {
        "timeSecond": "1688639403",
        "timeNano": "1688639403423213947"
    },
    "retExtinfo
": {},
    "time": 1688639403423
}
````