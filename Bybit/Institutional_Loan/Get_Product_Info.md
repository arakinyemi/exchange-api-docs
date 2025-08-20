# Get Product info

tip

When queried without an API key, this endpoint returns public product data

If your UID is bound with an OTC loan, then you can get your private product data by calling with your API key

If your UID is not bound with an OTC loan but you passed your API key, this endpoint returns public product data

HTTP Request
```http
GET /v5/ins-loan/product-infos
```

Request Parameters
| Parameter | Required | Type | Comments |
| --------- | -------- | ---- | -------- |
|productId	|false|	string	|Product ID. If not passed, returns all products

Response Parameters

| Parameter                 | Type    | Comments                                                                                                                                                                                                          |
|---------------------------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| marginProductInfo         | array   | Object                                                                                                                                                                                                            |
| > productId               | string  | Product ID                                                                                                                                                                                                        |
| > leverage                | string  | The maximum leverage for this loan product                                                                                                                                                                        |
| > supportSpot             | integer | Whether or not Spot is supported. 0:false; 1:true                                                                                                                                                                 |
| > supportContract         | integer | Whether USDT Perpetuals are supported. 0:false; 1:true                                                                                                                                                            |
| > supportMarginTrading    | integer | Whether or not Spot margin trading is supported. 0:false; 1:true                                                                                                                                                  |
| > deferredLiquidationLine | string  | Line for deferred liquidation                                                                                                                                                                                     |
| > deferredLiquidationTime | string  | Time for deferred liquidation                                                                                                                                                                                     |
| > withdrawLine            | string  | Restrict line for withdrawal                                                                                                                                                                                      |
| > transferLine            | string  | Restrict line for transfer                                                                                                                                                                                        |
| > spotBuyLine             | string  | Restrict line for Spot buy                                                                                                                                                                                        |
| > spotSellLine            | string  | Restrict line for Spot trading                                                                                                                                                                                    |
| > contractOpenLine        | string  | Restrict line for USDT Perpetual open position                                                                                                                                                                    |
| > liquidationLine         | string  | Line for liquidation                                                                                                                                                                                              |
| > stopLiquidationLine     | string  | Line for stop liquidation                                                                                                                                                                                         |
| > contractLeverage        | string  | The allowed default leverage for USDT Perpetual                                                                                                                                                                   |
| > transferRatio           | string  | The transfer ratio for loan funds to transfer from Spot wallet to Contract wallet                                                                                                                                 |
| > spotSymbols             | array   | The whitelist of spot trading pairs<br>If supportSpot="0", then it returns "[]"<br>If empty array, then you can trade any symbols<br>If not empty, then you can only trade listed symbols                         |
| > contractSymbols         | array   | The whitelist of contract trading pairs<br>If supportContract="0", then it returns "[]"<br>If empty array, then you can trade any symbols<br>If not empty, then you can only trade listed symbols                 |
| > supportUSDCContract     | integer | Whether or not USDC contracts are supported. '0':false; '1':true                                                                                                                                                  |
| > supportUSDCOptions      | integer | Whether or not Options are supported. '0':false; '1':true                                                                                                                                                         |
| > USDTPerpetualOpenLine   | string  | Restrict line to open USDT Perpetual position                                                                                                                                                                     |
| > USDCContractOpenLine    | string  | Restrict line to open USDC Contract position                                                                                                                                                                      |
| > USDCOptionsOpenLine     | string  | Restrict line to open Option position                                                                                                                                                                             |
| > USDTPerpetualCloseLine  | string  | Restrict line to trade USDT Perpetual                                                                                                                                                                             |
| > USDCContractCloseLine   | string  | Restrict line to trade USDC Contract                                                                                                                                                                              |
| > USDCOptionsCloseLine    | string  | Restrict line to trade Option                                                                                                                                                                                     |
| > USDCContractSymbols     | array   | The whitelist of USDC contract trading pairs<br>If supportContract="0", then it returns "[]"<br>If no whitelist symbols, it is [], and you can trade any<br>If supportUSDCContract="0", it is []                  |
| > USDCOptionsSymbols      | array   | The whitelist of Option symbols<br>If supportContract="0", then it returns "[]"<br>If no whitelisted, it is [], and you can trade any<br>If supportUSDCOptions="0", it is []                                      |
| > marginLeverage          | string  | The allowable maximum leverage for Spot margin trading. If supportMarginTrading=0, then it returns ""                                                                                                             |
| > USDTPerpetualLeverage   | array   | Object<br>If supportContract="0", it is []<br>If no whitelist USDT perp symbols, it returns all trading symbols and leverage by default<br>If there are whitelist symbols, it return those whitelist data         |
| >> symbol                 | string  | Symbol name                                                                                                                                                                                                       |
| >> leverage               | string  | Maximum leverage                                                                                                                                                                                                  |
| > USDCContractLeverage    | array   | Object<br>If supportUSDCContract="0", it is []<br>If no whitelist USDC contract symbols, it returns all trading symbols and leverage by default<br>If there are whitelist symbols, it return those whitelist data |
| >> symbol                 | string  | Symbol name                                                                                                                                                                                                       |
| >> leverage               | string  | Maximum leverage                                                                                                                                                                                                  |

Request Example
HTTP
```http
GET /v5/ins-loan/product-infos?productId=91 HTTP/1.1
Host: api-testnet.bybit.com
```
Response Example
```json
{
    "retCode": 0,
    "retMsg": "",
    "result": {
        "marginProductInfo": [
            {
                "productId": "91",
                "leverage": "4.00000000",
                "supportSpot": 1,
                "supportContract": 0,
                "withdrawLine": "",
                "transferLine": "",
                "spotBuyLine": "",
                "spotSellLine": "",
                "contractOpenLine": "",
                "liquidationLine": "0.75",
                "stopLiquidationLine": "0.35000000",
                "contractLeverage": "0",
                "transferRatio": "0",
                "spotSymbols": [],
                "contractSymbols": [],
                "supportUSDCContract": 0,
                "supportUSDCOptions": 0,
                "USDTPerpetualOpenLine": "",
                "USDCContractOpenLine": "",
                "USDCOptionsOpenLine": "",
                "USDTPerpetualCloseLine": "",
                "USDCContractCloseLine": "",
                "USDCOptionsCloseLine": "",
                "USDCContractSymbols": [],
                "USDCOptionsSymbols": [],
                "marginLeverage": "0",
                "USDTPerpetualLeverage": [],
                "USDCContractLeverage": [],
                "deferredLiquidationLine":"",
                "deferredLiquidationTime":"",
            }
        ]
    },
    "retExtInfo": {},
    "time": 1689747746332
}
```
