### Account
**Error Code from 59000 to 59999**

| Error Code | HTTP Status Code | Error Message                                                                                                                                    |
|------------|------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 59000      | 200              | Settings failed. Close any open positions or orders before modifying settings.                                                                   |
| 59001      | 200              | Switching unavailable as you have borrowings.                                                                                                   |
| 59002      | 200              | Sub-account settings failed. Close any open positions, orders, or trading bots before modifying settings.                                       |
| 59004      | 200              | Only IDs with the same instrument type are supported                                                                                            |
| 59005      | 200              | When margin is manually transferred in isolated mode, the value of the asset intially allocated to the position must be greater than 10,000 USDT.|
| 59006      | 200              | This feature is unavailable and will go offline soon.                                                                                           |
| 59101      | 200              | Leverage can't be modified. Please cancel all pending isolated margin orders before adjusting the leverage.                                    |
| 59102      | 200              | Leverage exceeds the maximum limit. Please lower the leverage.                                                                                  |
| 59103      | 200              | Account margin is insufficient and leverage is too low. Please increase the leverage.                                                          |
| 59104      | 200              | The borrowed position has exceeded the maximum position of this leverage. Please lower the leverage.                                            |
| 59105      | 400              | Leverage can't be less than {0}. Please increase the leverage.                                                                                  |
| 59106      | 200              | The max available margin corresponding to your order tier is {param0}. Please adjust your margin and place a new order.                       |
| 59107      | 200              | Leverage can't be modified. Please cancel all pending cross-margin orders before adjusting the leverage.                                        |
| 59108      | 200              | Your account leverage is too low and has insufficient margins. Please increase the leverage.                                                   |
| 59109      | 200              | Account equity less than the required margin amount after adjustment. Please adjust the leverage.                                             |
| 59110      | 200              | The instrument corresponding to this {param0} does not support the tgtCcy parameter.                                                           |
| 59111      | 200              | Leverage query isn't supported in portfolio margin account mode                                                                                 |
| 59112      | 200              | You have isolated/cross pending orders. Please cancel them before adjusting your leverage                                                      |
| 59113      | 200              | According to local laws and regulations, margin trading service is not available in your region. If your citizenship is at a different region, please complete KYC2 verification. |
| 59114      | 200              | According to local laws and regulations, margin trading services are not available in your region.                                            |
| 59117      | 200              | Cannot select more than {param0} crypto types                                                                                                  |
| 59118      | 200              | Amount placed should greater than {param0}                                                                                                   |
| 59119      | 200              | One-click repay is temporarily unavailable. Try again later.                                                                                   |
| 59120      | 200              | One-click convert is temporarily unavailable. Try again later.                                                                                 |
| 59121      | 200              | This batch is still under processing, please wait patiently.                                                                                   |
| 59122      | 200              | This batch has been processed                                                                                                                 |
| 59123      | 200              | {param0} order amount must be greater than {param1}                                                                                           |
| 59124      | 200              | The order amount of {param0} must be less than {param1}.                                                                                       |
| 59125      | 200              | {param0} doesn't support the current operation.                                                                                               |
| 59132      | 200              | Unable to switch. Please close or cancel all open orders and refer to the pre-check endpoint to stop any incompatible bots.                   |
| 59133      | 200              | Unable to switch due to insufficient assets for the chosen account mode.                                                                      |
| 59134      | 200              | Unable to switch. Refer to the pre-check endpoint and close any incompatible positions.                                                       |
| 59135      | 200              | Unable to switch. Refer to the pre-check endpoint and adjust your trades from copy trading.                                                   |
| 59136      | 200              | Unable to switch. Pre-set leverage for all cross margin contract positions then try again.                                                    |
| 59137      | 200              | Lower leverage to {param0} or below for all cross margin contract positions and try again.                                                   |
| 59138      | 200              | Unable to switch due to a position tier check failure.                                                                                        |
| 59139      | 200              | Unable to switch due to a margin check failure.                                                                                              |
| 59140      | 200              | You can only repay with your collateral crypto.                                                                                            |
| 59141      | 200              | The minimum repayment amount is {param0}. Select more available crypto or increase your trading account balance.                            |
| 59142      | 200              | Instant repay failed. You can only repay borrowable crypto.                                                                                  |
| 59200      | 200              | Insufficient account balance.                                                                                                               |
| 59201      | 200              | Negative account balance.                                                                                                                    |
| 59202      | 200              | No access to max opening amount in cross positions for PM accounts.                                                                         |
| 59300      | 200              | Margin call failed. Position does not exist.                                                                                                |
| 59301      | 200              | Margin adjustment failed for exceeding the max limit.                                                                                       |
| 59302      | 200              | Margin adjustment failed due to pending close order. Please cancel any pending close orders.                                                |
| 59303      | 200              | Insufficient available margin, add margin or reduce the borrowing amount                                                                   |
| 59304      | 200              | Insufficient equity for borrowing. Keep enough funds to pay interest for at least one day.                                                 |
| 59305      | 200              | Use VIP loan first to set the VIP loan priority                                                                                             |
| 59306      | 200              | Your borrowing amount exceeds the max limit                                                                                                |
| 59307      | 200              | You are not eligible for VIP loans                                                                                                         |
| 59308      | 200              | Unable to repay VIP loan due to insufficient borrow limit                                                                                   |
| 59309      | 200              | Unable to repay an amount that exceeds the borrowed amount                                                                                 |
| 59310      | 200              | Your account does not support VIP loan                                                                                                     |
| 59311      | 200              | Setup cannot continue. An outstanding VIP loan exists.                                                                                    |
| 59312      | 200              | {currency} does not support VIP loans                                                                                                     |
| 59313      | 200              | Unable to repay. You haven't borrowed any ${ccy} (${ccyPair}) in Quick margin mode.                                                       |
| 59314      | 200              | The current user is not allowed to return the money because the order is not borrowed                                                      |
| 59315      | 200              | viploan is upgrade now. Wait for 10 minutes and try again                                                                                  |
| 59316      | 200              | The current user is not allowed to borrow coins because the currency is in the order in the currency borrowing application.              |
| 59317      | 200              | The number of pending orders that are using VIP loan for a single currency cannot be more than {maxNumber} (orders)                       |
| 59319      | 200              | You can’t repay your loan order because your funds are in use. Make them available for full repayment.                                    |
| 59401      | 200              | Holdings limit reached.                                                                                                                    |
| 59402      | 200              | No passed instIDs are in a live state. Please verify instIDs separately.                                                                 |
| 59410      | 200              | You can only borrow this crypto if it supports borrowing and borrowing is enabled.                                                        |
| 59411      | 200              | Manual borrowing failed. Your account's free margin is insufficient.                                                                     |
| 59412      | 200              | Manual borrowing failed. The amount exceeds your borrowing limit.                                                                        |
| 59413      | 200              | You didn't borrow this crypto. No repayment needed.                                                                                      |
| 59414      | 200              | Manual borrowing failed. The minimum borrowing limit is {param0}.                                                                        |
| 59500      | 200              | Only the API key of the main account has permission.                                                                                    |
| 59501      | 200              | Each account can create up to 50 API keys                                                                                               |
| 59502      | 200              | This note name already exists. Enter a unique API key note name                                                                         |
| 59503      | 200              | Each API key can bind up to 20 IP addresses                                                                                            |
| 59504      | 200              | Sub-accounts don't support withdrawals. Please use your main account for withdrawals.                                                  |
| 59505      | 200              | The passphrase format is incorrect.                                                                                                   |
| 59506      | 200              | API key doesn't exist.                                                                                                                |
| 59507      | 200              | The two accounts involved in a transfer must be 2 different sub-accounts under the same main account.                                 |
| 59508      | 200              | The sub account of {param0} is suspended.                                                                                            |
| 59509      | 200              | Account doesn't have permission to reset market maker protection (MMP) status.                                                       |
| 59510      | 200              | Sub-account does not exist                                                                                                           |
| 59512      | 200              | Unable to set up permissions for ND broker subaccounts. By default, all ND subaccounts can transfer funds out.                       |
| 59515      | 200              | You are currently not on the custody whitelist. Please contact customer service for assistance.                                      |
| 59516      | 200              | Please create the Copper custody funding account first.                                                                               |
| 59517      | 200              | Please create the Komainu custody funding account first.                                                                              |
| 59518      | 200              | You can’t create a sub-account using the API; please use the app or web.                                                             |
| 59519      | 200              | You can’t use this function/feature while it's frozen, due to: {freezereason}                                                       |
| 59601      | 200              | Subaccount name already exists.                                                                                                     |
| 59603      | 200              | Maximum number of subaccounts reached.                                                                                             |
| 59604      | 200              | Only the API key of the main account can access this API.                                                                           |
| 59606      | 200              | Failed to delete sub-account. Transfer all sub-account funds to your main account before deleting your sub-account.                 |
| 59608      | 200              | Only Broker accounts have permission to access this API.                                                                           |
| 59609      | 200              | Broker already exists                                                                                                               |
| 59610      | 200              | Broker does not exist                                                                                                               |
| 59611      | 200              | Broker unverified                                                                                                                  |
| 59612      | 200              | Cannot convert time format                                                                                                         |
| 59613      | 200              | No escrow relationship established with the subaccount.                                                                           |
| 59614      | 200              | Managed subaccount does not support this operation.                                                                               |
| 59615      | 200              | The time interval between the Begin Date and End Date cannot be greater than 180 days.                                             |
| 59616      | 200              | The Begin Date cannot be later than the End Date.                                                                                 |
| 59617      | 200              | Sub-account created. Account level setup failed.                                                                                  |
| 59618      | 200              | Failed to create sub-account.                                                                                                     |
| 59619      | 200              | This endpoint does not support ND sub accounts. Please use the dedicated endpoint supported for ND brokers.                       |
| 59622      | 200              | You're creating a sub-account for a non-existing or incorrect sub-account. Create a sub-account under the ND broker first or use the correct sub-account code. |
| 59623      | 200              | Couldn't delete the sub-account under the ND broker as the sub-account has one or more sub-accounts, which must be deleted first.    |
| 59648      | 200              | Your modified spot-in-use amount is insufficient, which may lead to liquidation. Adjust the amount.                               |
| 59649      | 200              | Disabling spot-derivatives risk offset mode may increase the risk of liquidation. Adjust the size of your positions and ensure your maintenance maintenance margin ratio is safe. |
| 59650      | 200              | Switching your offset unit may increase the risk of liquidation. Adjust the size of your positions and ensure your maintenance maintenance margin ratio is safe. |
| 59651      | 200              | Enable spot-derivatives risk offset mode to set your spot-in-use amount.                                                         |
| 59652      | 200              | You can only set a spot-in-use amount for crypto that can be used as margin.                                                     |
| 59658      | 200              | {ccy} isn’t supported as collateral.                                                                                            |
| 59658      | 200              | {ccy} and {ccy1} aren’t supported as collateral.                                                                               |
| 59658      | 200              | {ccy}, {ccy1}, and {ccy2} aren’t supported as collateral.                                                                      |
| 59658      | 200              | {ccy}, {ccy1}, {ccy2}, and {number} other crypto aren’t supported as collateral.                                               |
| 59659      | 200              | Failed to apply settings because you must also enable {ccy} to enable {ccy1} as collateral.                                      |
| 59660      | 200              | Failed to apply settings because you must also disable {ccy} to disable {ccy1} as collateral.                                    |
| 59661      | 200              | Failed to apply settings because you can’t disable {ccy} as collateral.                                                         |
| 59662      | 200              | Failed to apply settings because of open orders or positions requiring {ccy} as collateral.                                      |
| 59662      | 200              | Failed to apply settings because of open orders or positions requiring {ccy} and {ccy1} as collateral.                            |
| 59662      | 200              | Failed to apply settings because of open orders or positions requiring {ccy}, {ccy1}, and {ccy2} as collateral.                   |
| 59662      | 200              | Failed to apply settings because of open orders or positions requiring {ccy}, {ccy1}, {ccy2}, and {number} other crypto as collateral. |
| 59664      | 200              | Failed to apply settings because you have borrowings in {ccy}.                                                                  |
| 59664      | 200              | Failed to apply settings because you have borrowings in {ccy} and {ccy1}.                                                       |
| 59664      | 200              | Failed to apply settings because you have borrowings in {ccy}, {ccy1}, and {ccy2}.                                              |
| 59664      | 200              | Failed to apply settings because you have borrowings in {ccy}, {ccy1}, {ccy2}, and {number} other crypto.                         |
| 59665      | 200              | Failed to apply settings. Enable other cryptocurrencies as collateral to meet the position’s margin requirements.                |
| 59666      | 200              | Failed to apply settings because you can’t enable and disable a crypto as collateral at the same time.                            |
| 59668      | 200              | Cancel isolated margin TP/SL, trailing, trigger, and chase orders or stop bots before adjusting your leverage.                    |
| 59669      | 200              | Cancel cross-margin TP/SL, trailing, trigger, and chase orders or stop bots before adjusting your leverage.                       |
| 59670      | 200              | You have more than {param0} open orders for this trading pair. Cancel to reduce your orders to {param1} or fewer before adjusting your leverage. |
| 59671      | 200              | Auto-earn currently doesn’t support {param0}.                                                                                    |
| 59672      | 200              | You can’t modify your minimmum lending APR when Auto-earn is off.                                                                |
| 59673      | 200              | You can’t turn off Auto-earn within 24 hours of turning it on. Try again at {param0}.                                             |
| 59674      | 200              | You can’t borrow to transfer or withdraw when Auto-earn is on for this cryptocurrency.                                             |
| 59675      | 200              | You’ve already turned on Auto-earn for {param0}.                                                                                  |
| 59676      | 200              | You can only use Auto-earn if your trading fee tier is {param0} or higher.                                                        |

***
