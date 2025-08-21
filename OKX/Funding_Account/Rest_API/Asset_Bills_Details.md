### Asset bills details
Query the billing record in the past month.

**Rate Limit:** 6 Requests per second  
**Rate limit rule:** User ID  
**Permission:** Read

HTTP Request
```
GET /api/v5/asset/bills
```

Request Example
```
GET /api/v5/asset/bills
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | No | Currency |
| type | String | No | Bill type<br>1: Deposit<br>2: Withdrawal<br>13: Canceled withdrawal<br>20: Transfer to sub account (for master account)<br>21: Transfer from sub account (for master account)<br>22: Transfer out from sub to master account (for sub-account)<br>23: Transfer in from master to sub account (for sub-account)<br>28: Manually claimed Airdrop<br>47: System reversal<br>48: Event Reward<br>49: Event Giveaway<br>68: Fee rebate (by rebate card)<br>72: Token received<br>73: Token given away<br>74: Token refunded<br>75: [Simple earn flexible] Subscription<br>76: [Simple earn flexible] Redemption<br>77: Jumpstart distribute<br>78: Jumpstart lock up<br>80: DEFI/Staking subscription<br>82: DEFI/Staking redemption<br>83: Staking yield<br>84: Violation fee<br>89: Deposit yield<br>116: [Fiat] Place an order<br>117: [Fiat] Fulfill an order<br>118: [Fiat] Cancel an order<br>124: Jumpstart unlocking<br>130: Transferred from Trading account<br>131: Transferred to Trading account<br>132: [P2P] Frozen by customer service<br>133: [P2P] Unfrozen by customer service<br>134: [P2P] Transferred by customer service<br>135: Cross chain exchange<br>137: [ETH Staking] Subscription<br>138: [ETH Staking] Swapping<br>139: [ETH Staking] Earnings<br>146: Customer feedback<br>150: Affiliate commission<br>151: Referral reward<br>152: Broker reward<br>160: Dual Investment subscribe<br>161: Dual Investment collection<br>162: Dual Investment profit<br>163: Dual Investment refund<br>172: [Affiliate] Sub-affiliate commission<br>173: [Affiliate] Fee rebate (by trading fee)<br>174: Jumpstart Pay<br>175: Locked collateral<br>176: Loan<br>177: Added collateral<br>178: Returned collateral<br>179: Repayment<br>180: Unlocked collateral<br>181: Airdrop payment<br>185: [Broker] Convert reward<br>187: [Broker] Convert transfer<br>189: Mystery box bonus<br>195: Untradable asset withdrawal<br>196: Untradable asset withdrawal revoked<br>197: Untradable asset deposit<br>198: Untradable asset collection reduce<br>199: Untradable asset collection increase<br>200: Buy<br>202: Price Lock Subscribe<br>203: Price Lock Collection<br>204: Price Lock Profit<br>205: Price Lock Refund<br>207: Dual Investment Lite Subscribe<br>208: Dual Investment Lite Collection<br>209: Dual Investment Lite Profit<br>210: Dual Investment Lite Refund<br>212: [Flexible loan] Multi-collateral loan collateral locked<br>215: [Flexible loan] Multi-collateral loan collateral released<br>217: [Flexible loan] Multi-collateral loan borrowed<br>218: [Flexible loan] Multi-collateral loan repaid<br>232: [Flexible loan] Subsidized interest received<br>220: Delisted crypto<br>221: Blockchain's withdrawal fee<br>222: Withdrawal fee refund<br>223: SWAP lead trading profit share<br>225: Shark Fin subscribe<br>226: Shark Fin collection<br>227: Shark Fin profit<br>228: Shark Fin refund<br>229: Airdrop<br>232: Subsidized interest received<br>233: Broker rebate compensation<br>240: Snowball subscribe<br>241: Snowball refund<br>242: Snowball profit<br>243: Snowball trading failed<br>249: Seagull subscribe<br>250: Seagull collection<br>251: Seagull profit<br>252: Seagull refund<br>263: Strategy bots profit share<br>265: Signal revenue<br>266: SPOT lead trading profit share<br>270: DCD broker transfer<br>271: DCD broker rebate<br>272: [Convert] Buy Crypto/Fiat<br>273: [Convert] Sell Crypto/Fiat<br>284: [Custody] Transfer out trading sub-account<br>285: [Custody] Transfer in trading sub-account<br>286: [Custody] Transfer out custody funding account<br>287: [Custody] Transfer in custody funding account<br>288: [Custody] Fund delegation<br>289: [Custody] Fund undelegation<br>299: Affiliate recommendation commission<br>300: Fee discount rebate<br>303: Snowball market maker transfer<br>304: [Simple Earn Fixed] Order submission<br>305: [Simple Earn Fixed] Order redemption<br>306: [Simple Earn Fixed] Principal distribution<br>307: [Simple Earn Fixed] Interest distribution (early termination compensation)<br>308: [Simple Earn Fixed] Interest distribution<br>309: [Simple Earn Fixed] Interest distribution (extension compensation)<br>311: Crypto dust auto-transfer in<br>313: Sent by gift<br>314: Received from gift<br>315: Refunded from gift<br>328: [SOL staking] Send Liquidity Staking Token reward<br>329: [SOL staking] Subscribe Liquidity Staking Token staking<br>330: [SOL staking] Mint Liquidity Staking Token<br>331: [SOL staking] Redeem Liquidity Staking Token order<br>332: [SOL staking] Settle Liquidity Staking Token order<br>333: Trial fund reward<br>339: [Simple Earn Fixed] Order submission<br>340: [Simple Earn Fixed] Order failure refund<br>341: [Simple Earn Fixed] Redemption<br>342: [Simple Earn Fixed] Principal<br>343: [Simple Earn Fixed] Interest<br>344: [Simple Earn Fixed] Compensatory interest<br>345: [Institutional Loan] Principal repayment<br>346: [Institutional Loan] Interest repayment<br>347: [Institutional Loan] Overdue penalty<br>348: [BTC staking] Subscription<br>349: [BTC staking] Redemption<br>350: [BTC staking] Earnings<br>351: [Institutional Loan] Loan disbursement<br>354: Copy and bot rewards<br>361: Deposit from closed sub-account<br>372: Asset segregation<br>373: Asset release |
| clientId | String | No | Client-supplied ID for transfer or withdrawal<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| after | String | No | Pagination of data to return records earlier than the requested ts or billId, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| before | String | No | Pagination of data to return records newer than the requested ts or billId, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| limit | String | No | Number of results per request. The maximum is 100. The default is 100. |
| pagingType | String | No | PagingType<br>1: Timestamp of the bill record<br>2: Bill ID of the bill record<br>The default is 1 |

Response Example
```
{
    "code": "0",
    "msg": "",
    "data": [{
        "billId": "12344",
        "ccy": "BTC",
        "clientId": "",
        "balChg": "2",
        "bal": "12",
        "type": "1",
        "ts": "1597026383085",
        "notes": ""
    }]
}
```

Response Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| billId | String | Bill ID |
| ccy | String | Account balance currency |
| clientId | String | Client-supplied ID for transfer or withdrawal |
| balChg | String | Change in balance at the account level |
| bal | String | Balance at the account level |
| type | String | Bill type |
| notes | String | Notes |
| ts | String | Creation time, Unix timestamp format in milliseconds, e.g.1597026383085 |
