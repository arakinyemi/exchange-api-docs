# Get Exchange Entity List
This endpoint is particularly used for kyc=KOR users. When withdraw funds, you need to fill entity id.


HTTP Request
```http
GET /v5/asset/withdraw/vasp/list
```

Request Parameters
None



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| vasp | array | Exchange entity info |
| > vaspEntityId | string |	Receiver platform id. When transfer to Upbit or other exchanges that not in the list, please use vaspEntityId='others' |
| > vaspName  | string |	Receiver platform name

---

Request Example

HTTP
 
  
```http
GET /v5/asset/withdraw/vasp/list HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1715067106163
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXXX
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "vasp": [
            {
                "vaspEntityId": "basic-finance",
                "vaspName": "Basic-finance"
            },
            {
                "vaspEntityId": "beeblock",
                "vaspName": "Beeblock"
            },
            {
                "vaspEntityId": "bithumb",
                "vaspName": "bithumb"
            },
            {
                "vaspEntityId": "cardo",
                "vaspName": "cardo"
            },
            {
                "vaspEntityId": "codevasp",
                "vaspName": "codevasp"
            },
            {
                "vaspEntityId": "codexchange-kor",
                "vaspName": "CODExchange-kor"
            },
            {
                "vaspEntityId": "coinone",
                "vaspName": "coinone"
            },
            {
                "vaspEntityId": "dummy",
                "vaspName": "Dummy"
            },
            {
                "vaspEntityId": "flata-exchange",
                "vaspName": "flataexchange"
            },
            {
                "vaspEntityId": "fobl",
                "vaspName": "Foblgate"
            },
            {
                "vaspEntityId": "hanbitco",
                "vaspName": "hanbitco"
            },
            {
                "vaspEntityId": "hexlant",
                "vaspName": "hexlant"
            },
            {
                "vaspEntityId": "inex",
                "vaspName": "INEX"
            },
            {
                "vaspEntityId": "infiniteblock-corp",
                "vaspName": "InfiniteBlock Corp"
            },
            {
                "vaspEntityId": "kdac",
                "vaspName": "kdac"
            },
            {
                "vaspEntityId": "korbit",
                "vaspName": "korbit"
            },
            {
                "vaspEntityId": "paycoin",
                "vaspName": "Paycoin"
            },
            {
                "vaspEntityId": "qbit",
                "vaspName": "Qbit"
            },
            {
                "vaspEntityId": "tennten",
                "vaspName": "TENNTEN"
            },
            {
                "vaspEntityId": "others",
                "vaspName": "Others (including Upbit)"
            }
        ]
    },
    "retExtinfo
": {},
    "time": 1715067106537
}
```

