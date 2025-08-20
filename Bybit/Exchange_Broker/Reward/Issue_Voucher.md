# Issue Voucher

HTTP Request
```http
POST /v5/broker/award/distribute-award
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| accountId | true | string | User ID |
| awardId | true | string | Voucher ID |
| specCode | true | string | Customised unique spec code, up to 8 characters |
| amount | true | string | Issue amount <br> Spot airdrop supports up to 16 decimals <br> Other types supports up to 4 decimals |
| brokerId | true | string | Broker ID |

---


Response Parameters
None


Request Example

HTTP
 
  
```http
POST /v5/broker/award/distribute-award HTTP/1.1
Host: api.bybit.com
X-BAPI-SIGN: XXXXXX
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1726110531734
X-BAPI-RECV-WINDOW: 5000
Content-Type: application/json
Content-Length: 128
```

```json
{
    "accountId": "2846381",
    "awardId": "123456",
    "specCode": "award-001",
    "amount": "100",
    "brokerId": "v-28478"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": ""
}
```

