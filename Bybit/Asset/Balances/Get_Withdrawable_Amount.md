# Get Withdrawable Amount
info

How can partial funds be subject to delayed withdrawal requests?

On-chain deposit: If the number of on-chain confirmations has not reached a risk-controlled level, a portion of the funds will be frozen for a period of time until they are unfrozen.
Buying crypto: If there is a risk, the funds will be frozen for a certain period of time and cannot be withdrawn.

HTTP Request
```http
GET /v5/asset/withdraw/withdrawable-amount
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
| coin | true | string | Coin name, uppercase only |

---


Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| limitAmountUsd | string | The frozen amount due to risk, in USD |
| withdrawableAmount | Object |
| > SPOT | Object | Spot wallet, it is not returned if spot wallet is removed |
| >> coin | string | Coin name |
| >> withdrawableAmount | string | Amount that can be withdrawn |
| >> availableBalance | string | Available balance |
| > FUND | Object | Funding wallet |
| >> coin | string | Coin name |
| >> withdrawableAmount | string | Amount that can be withdrawn |
| >> availableBalance | string | Available balance |
| > UTA | Object | Unified wallet |
| >> coin | string | Coin name |
| >> withdrawableAmount | string | Amount that can be withdrawn |
| >> availableBalance | string | Available balance |

---

Request Example

HTTP
 
  
```http
GET /v5/asset/withdraw/withdrawable-amount?coin=USDT HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1677565621998
X-BAPI-RECV-WINDOW: 50000
X-BAPI-SIGN: XXXXXX
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "limitAmountUsd": "595051.7",
        "withdrawableAmount": {
            "FUND": {
                "coin": "USDT",
                "withdrawableAmount": "155805.847",
                "availableBalance": "155805.847"
            },
            "UTA": {
                "coin": "USDT",
                "withdrawableAmount": "498751.0882",
                "availableBalance": "498751.0882"
            }
        }
    },
    "retExtinfo
": {},
    "time": 1754009688289
}
```

