### Fill ratio based sub-account rate limit
This is only applicable to >= VIP5 customers.
As an incentive for more efficient trading, the exchange will offer a higher sub-account rate limit to clients with a high trade fill ratio.

The exchange calculates two ratios based on the transaction data from the past 7 days at 00:00 UTC.

Sub-account fill ratio: This ratio is determined by dividing (the trade volume in USDT of the sub-account) by (sum of (new and amendment request count per symbol * symbol multiplier) of the sub-account). Note that the master trading account itself is also considered as a sub-account in this context.
Master account aggregated fill ratio: This ratio is calculated by dividing (the trade volume in USDT on the master account level) by (the sum (new and amendment count per symbol * symbol multiplier] of all sub-accounts).


The symbol multiplier allows for fine-tuning the weight of each symbol. A smaller symbol multiplier (<1) is used for smaller pairs that require more updates per trading volume. All instruments have a default symbol multiplier, and some instruments will have overridden symbol multipliers.
| InstType           | Override rule          | Overridden symbol multiplier | Default symbol multiplier |
|-------------------|----------------------|-----------------------------|--------------------------|
| Perpetual Futures  | Per instrument ID     | 1                           |                           |
| Instrument ID:     |                      |                             |                          |
| BTC-USDT-SWAP      |                      | 0.2                         |                          |
| BTC-USD-SWAP       |                      | 0.2                         |                          |
| ETH-USDT-SWAP      |                      | 0.2                         |                          |
| ETH-USD-SWAP       |                      | 0.2                         |                          |
| Expiry Futures     | Per instrument Family | 0.3                         | 0.1                      |
| Instrument Family: |                      |                             |                          |
| BTC-USDT           |                      | 0.1                         |                          |
| BTC-USD            |                      | 0.1                         |                          |
| ETH-USDT           |                      | 0.1                         |                          |
| ETH-USD            |                      | 0.1                         |                          |
| Spot               | Per instrument ID     | 0.5                         | 0.1                      |
| Instrument ID:     |                      |                             |                          |
| BTC-USDT           |                      | 0.1                         |                          |
| ETH-USDT           |                      | 0.1                         |                          |
| Options            | Per instrument Family |                             | 0.1                      |

The fill ratio computation excludes block trading, spread trading, MMP and fiat orders for order count; and excludes block trading, spread trading for trade volume. Only successful order requests (sCode=0) are considered.




At 08:00 UTC, the system will use the maximum value between the sub-account fill ratio and the master account aggregated fill ratio based on the data snapshot at 00:00 UTC to determine the sub-account rate limit based on the table below. For broker (non-disclosed) clients, the system considers the sub-account fill ratio only.
| Tier   | Fill Ratio `[x <= ratio < y)` | Sub-account Rate Limit per 2s (new & amendment) |
| ------ | ----------------------------- | ----------------------------------------------- |
| Tier 1 | \[0,1)                        | 1,000                                           |
| Tier 2 | \[1,2)                        | 1,250                                           |
| Tier 3 | \[2,3)                        | 1,500                                           |
| Tier 4 | \[3,5)                        | 1,750                                           |
| Tier 5 | \[5,10)                       | 2,000                                           |
| Tier 6 | \[10,20)                      | 2,500                                           |
| Tier 7 | \[20,50)                      | 3,000                                           |
| Tier 8 | >=50                          | 10,000                                          |

If there is an improvement in the fill ratio and rate limit to be uplifted, the uplift will take effect immediately at 08:00 UTC. However, if the fill ratio decreases and the rate limit needs to be lowered, a one-day grace period will be granted, and the lowered rate limit will only be implemented on T+1 at 08:00 UTC. On T+1, if the fill ratio improves, the higher rate limit will be applied accordingly. In the event of client demotion to VIP4, their rate limit will be downgraded to Tier 1, accompanied by a one-day grace period.



If the 7-day trading volume of a sub-account is less than 1,000,000 USDT, the fill ratio of the master account will be applied to it.



For newly created sub-accounts, the Tier 1 rate limit will be applied at creation until T+1 8am UTC, at which the normal rules will be applied.



Block trading, spread trading, MMP and spot/margin orders are exempted from the sub-account rate limit.



The exchange offers GET / Account rate limit endpoint that provides ratio and rate limit data, which will be updated daily at 8am UTC. It will return the sub-account fill ratio, the master account aggregated fill ratio, current sub-account rate limit and sub-account rate limit on T+1 (applicable if the rate limit is going to be demoted).

The fill ratio and rate limit calculation example is shown below. Client has 3 accounts, symbol multiplier for BTC-USDT-SWAP = 1 and XRP-USDT = 0.1.

Account A (master account):
BTC-USDT-SWAP trade volume = 100 USDT, order count = 10;
XRP-USDT trade volume = 20 USDT, order count = 15;
Sub-account ratio = (100+20) / (10 * 1 + 15 * 0.1) = 10.4
Account B (sub-account):
BTC-USDT-SWAP trade volume = 200 USDT, order count = 100;
XRP-USDT trade volume = 20 USDT, order count = 30;
Sub-account ratio = (200+20) / (100 * 1 + 30 * 0.1) = 2.13
Account C (sub-account):
BTC-USDT-SWAP trade volume = 300 USDT, order count = 1000;
XRP-USDT trade volume = 20 USDT, order count = 45;
Sub-account ratio = (300+20) / (100 * 1 + 45 * 0.1) = 3.06
Master account aggregated fill ratio = (100+20+200+20+300+20) / (10 * 1 + 15 * 0.1 + 100 * 1 + 30 * 0.1 + 100 * 1 + 45 * 0.1) = 3.01
Rate limit of accounts
Account A = max(10.4, 3.01) = 10.4 -> 2500 order requests/2s
Account B = max(2.13, 3.01) = 3.01 -> 1750 order requests/2s
Account C = max(3.06, 3.01) = 3.06 -> 1750 order requests/2s
Best practices
If you require a higher request rate than our rate limit, you can set up different sub-accounts to batch request rate limits. We recommend this method for throttling or spacing out requests in order to maximize each accounts' rate limit and avoid disconnections or rejections.
