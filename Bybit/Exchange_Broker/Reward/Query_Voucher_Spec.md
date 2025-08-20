# Query Voucher Spec

HTTP Request
```http
POST /v5/broker/award/info
```


Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| id | true | string | Voucher ID |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| id | string | Voucher ID |
| coin | string | Coin |
| amountUnit | string | AWARD_AMOUNT_UNIT_USD <br> AWARD_AMOUNT_UNIT_COIN |
| productLine | string | Product line |
| subProductLine | string | Sub product line |
| totalAmount | Object | Total amount of voucher |
| usedAmount | string | Used amount of voucher |

---

Request Example

HTTP
 
  
```http
POST /v5/broker/award/info
 HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1726107086048
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 22
```

```json
{
    "id": "80209"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "id": "80209",
        "coin": "USDT",
        "amountUnit": "AWARD_AMOUNT_UNIT_USD",
        "productLine": "PRODUCT_LINE_CONTRACT",
        "subProductLine": "SUB_PRODUCT_LINE_CONTRACT_DEFAULT",
        "totalAmount": "10000",
        "usedAmount": "100"
    },
    "retExtinfo
": {},
    "time": 1726107086313
}
```

