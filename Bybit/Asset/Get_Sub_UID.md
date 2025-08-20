# Get Sub UID
Query the sub UIDs under a main UID. It returns up to 2000 sub accounts, if you need more, please call this endpoint.

info

Query by the master UID's api key only


HTTP Request
```http
GET /v5/asset/transfer/query-sub-member-list
```

Request Parameters
None



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| subMemberIds | array<string> | All sub UIDs under the main UID |
| transferableSubMemberIds | array<string> | All sub UIDs that have universal transfer enabled |

---


Request Example

HTTP
 
  
```http
GET /v5/asset/transfer/query-sub-member-list HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-SIGN: XXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672147239931
X-BAPI-RECV-WINDOW: 5000
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "subMemberIds": [
            "554117",
            "592324",
            "592334",
            "1055262",
            "1072055",
            "1119352"
        ],
        "transferableSubMemberIds": [
            "554117",
            "592324"
        ]
    },
    "retExtinfo
": {},
    "time": 1672147241320
}
```

