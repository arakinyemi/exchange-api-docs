# Wallet

Subscribe to the wallet stream to see changes to your wallet in real-time.

> **info**  
> There is no snapshot event given at the time when the subscription is successful  
> Unrealised PnL change does not trigger an event.

**Topic:** wallet

### Response Parameters

| Parameter          | Type    | Comments                                                                        |
|--------------------|---------|---------------------------------------------------------------------------------|
| id                 | string  | Message ID                                                                      |
| topic              | string  | Topic name                                                                      |
| creationTime       | number  | Data created timestamp (ms)                                                     |
| data               | array   | Object                                                                          |

Inside `data` array objects:

| Parameter               | Type       | Comments                                                                                          |
|-------------------------|------------|-------------------------------------------------------------------------------------------------|
| accountType             | string     | Account type  <br>UTA2.0: UNIFIED  <br>UTA1.0: UNIFIED (spot/linear/options), CONTRACT (inverse)  <br>Classic: CONTRACT, SPOT                                         |
| accountLTV              | string     | Deprecated field                                                                                |
| accountIMRate           | string     | Account IM rate. Not applicable for isolated margin UTA2.0/UTA1.0/Classic(SPOT, CONTRACT)      |
| accountIMRateByMp       | string     | Account IM rate calculated by mark price. Same note as accountIMRate                            |
| accountMMRate           | string     | Account MM rate                                                                                |
| accountMMRateByMp       | string     | Account MM rate calculated by mark price                                                       |
| totalEquity             | string     | Account total equity (USD)                                                                      |
| totalWalletBalance      | string     | Account wallet balance (USD): sum of asset wallet balance by USD value of each asset           |
| totalMarginBalance      | string     | totalWalletBalance + totalPerpUPL                                                              |
| totalAvailableBalance   | string     | Account available balance (USD), Cross Margin: totalMarginBalance - totalInitialMargin          |
| totalPerpUPL            | string     | Account Perps and Futures unrealised P&L (USD): sum of each Perp and USDC Futures UPL by base coin|
| totalInitialMargin      | string     | Account initial margin (USD): sum of asset total initial margin base coin                       |
| totalInitialMarginByMp  | string     | Account initial margin (USD) calculated by mark price                                           |
| totalMaintenanceMargin  | string     | Account maintenance margin (USD): sum of asset total maintenance margin base coin               |
| totalMaintenanceMarginByMp | string  | Account maintenance margin (USD) calculated by mark price                                      |
| coin                    | array      | Coin objects                                                                                   |

`coin` array objects contain:

| Parameter           | Type    | Comments                                                                                         |
|---------------------|---------|------------------------------------------------------------------------------------------------|
| coin                | string  | Coin name, e.g. BTC, ETH, USDT, USDC                                                           |
| equity              | string  | Equity of coin                                                                                  |
| usdValue            | string  | USD value of coin. If coin cannot be collateral, then 0                                        |
| walletBalance       | string  | Wallet balance of coin                                                                          |
| free                | string  | Available balance for Spot wallet. Unique for Classic SPOT                                     |
| locked              | string  | Locked balance due to Spot open order                                                          |
| spotHedgingQty      | string  | Spot asset quantity used to hedge in portfolio margin, truncated to 8 decimals, default "0"    |
| borrowAmount        | string  | Borrow amount of coin                                                                          |
| availableToBorrow   | string  | Deprecated field, always returns "" due to main-sub UID sharing borrow quota. See Get Collateral Info  |
| availableToWithdraw  | string  | Available amount to withdraw. Deprecated for UNIFIED account. Use Get Transferable Amount instead |
| accruedInterest     | string  | Accrued interest                                                                             |
| totalOrderIM        | string  | Pre-occupied margin for order. Returns "" for portfolio margin mode                            |
| totalPositionIM     | string  | Sum of initial margin of all positions + pre-occupied liquidation fee. Returns "" for portfolio margin mode |
| totalPositionMM     | string  | Sum of maintenance margin for all positions. Returns "" for portfolio margin mode            |
| unrealisedPnl       | string  | Unrealised PnL                                                                             |
| cumRealisedPnl     | string  | Cumulative realised PnL                                                                    |
| bonus               | string  | Bonus. Unique for UNIFIED account                                                            |
| collateralSwitch    | boolean | Whether can be used as collateral currency (platform)                                        |
| marginCollateral    | boolean | Whether collateral is turned on by user (user)                                              |

### Subscribe Example

```json
{
    "op": "subscribe",
    "args": [
        "wallet"
    ]
}
```

```python
from pybit.unified_trading import WebSocket
from time import sleep

ws = WebSocket(
    testnet=True,
    channel_type="private",
    api_key="xxxxxxxxxxxxxxxxxx",
    api_secret="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
)

def handle_message(message):
    print(message)

ws.wallet_stream(callback=handle_message)

while True:
    sleep(1)
```

### Stream Example

```json
{
    "id": "592324d2bce751-ad38-48eb-8f42-4671d1fb4d4e",
    "topic": "wallet",
    "creationTime": 1700034722104,
    "data": [
        {
            "accountIMRate": "0",
            "accountIMRateByMp": "0",
            "accountMMRate": "0",
            "accountMMRateByMp": "0",
            "totalEquity": "10262.91335023",
            "totalWalletBalance": "9684.46297164",
            "totalMarginBalance": "9684.46297164",
            "totalAvailableBalance": "9556.6056555",
            "totalPerpUPL": "0",
            "totalInitialMargin": "0",
            "totalInitialMarginByMp": "0",
            "totalMaintenanceMargin": "0",
            "totalMaintenanceMarginByMp": "0",
            "coin": [
                {
                    "coin": "BTC",
                    "equity": "0.00102964",
                    "usdValue": "36.70759517",
                    "walletBalance": "0.00102964",
                    "availableToWithdraw": "0.00102964",
                    "availableToBorrow": "",
                    "borrowAmount": "0",
                    "accruedInterest": "0",
                    "totalOrderIM": "",
                    "totalPositionIM": "",
                    "totalPositionMM": "",
                    "unrealisedPnl": "0",
                    "cumRealisedPnl": "-0.00000973",
                    "bonus": "0",
                    "collateralSwitch": true,
                    "marginCollateral": true,
                    "locked": "0",
                    "spotHedgingQty": "0.01592413"
                }
            ],
            "accountLTV": "0",
            "accountType": "UNIFIED"
        }
    ]
}
```


