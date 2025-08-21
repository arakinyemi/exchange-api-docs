### Funding
**Error Code from 58000 to 58999**

| Error Code | HTTP Status Code | Error Message                                                                                                                                    |
|------------|------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 58002      | 200              | Please activate Savings Account first.                                                                                                          |
| 58003      | 200              | Savings does not support this currency type                                                                                                     |
| 58004      | 200              | Account blocked.                                                                                                                                 |
| 58005      | 200              | The {behavior} amount must be equal to or less than {minNum}                                                                                     |
| 58006      | 200              | Service unavailable for token {0}.                                                                                                              |
| 58007      | 200              | Assets interface is currently unavailable. Try again later                                                                                      |
| 58008      | 200              | You do not have assets in this currency.                                                                                                       |
| 58009      | 200              | Crypto pair doesn't exist                                                                                                                       |
| 58010      | 200              | Chain {chain} isn't supported                                                                                                                   |
| 58011      | 200              | Due to local laws and regulations, our services are unavailable to unverified users in {region}. Please verify your account.                     |
| 58012      | 200              | Due to local laws and regulations, OKX does not support asset transfers to unverified users in {region}. Please make sure your recipient has a verified account. |
| 58013      | 200              | Withdrawals not supported yet, contact customer support for details                                                                             |
| 58014      | 200              | Deposits not supported yet, contact customer support for details                                                                                |
| 58015      | 200              | Transfers not supported yet, contact customer support for details                                                                               |
| 58016      | 200              | The API can only be accessed and used by the trading team's main account                                                                       |
| 58100      | 200              | The trading product triggers risk control, and the platform has suspended the fund transfer-out function with related users. Please wait patiently. |
| 58101      | 200              | Transfer suspended                                                                                                                             |
| 58102      | 429              | Rate limit reached. Please refer to API docs and throttle requests accordingly.                                                               |
| 58103      | 200              | This account transfer function is temporarily unavailable. Please contact customer service for details.                                       |
| 58104      | 200              | Since your P2P transaction is abnormal, you are restricted from making fund transfers. Please contact customer support to remove the restriction. |
| 58105      | 200              | Since your P2P transaction is abnormal, you are restricted from making fund transfers. Please transfer funds on our website or app to complete identity verification. |
| 58106      | 200              | USD verification failed.                                                                                                                       |
| 58107      | 200              | Crypto verification failed.                                                                                                                    |
| 58110      | 200              | Transfers are suspended due to market risk control triggered by your {businessType} {instFamily} trades or positions. Please try again in a few minutes. Contact customer support if further assistance is needed. |
| 58111      | 200              | Fund transfers are unavailable while perpetual contracts are charging funding fees. Try again later.                                          |
| 58112      | 200              | Transfer failed. Contact customer support for assistance                                                                                       |
| 58113      | 200              | Unable to transfer this crypto                                                                                                                 |
| 58114      | 400              | Transfer amount must be greater than 0                                                                                                        |
| 58115      | 200              | Sub-account does not exist.                                                                                                                    |
| 58116      | 200              | Transfer exceeds the available amount.                                                                                                        |
| 58117      | 200              | Transfer failed. Resolve any negative assets before transferring again                                                                         |
| 58119      | 200              | {0} Sub-account has no permission to transfer out, please set first.                                                                         |
| 58120      | 200              | Transfers are currently unavailable. Try again later                                                                                           |
| 58121      | 200              | This transfer will result in a high-risk level of your position, which may lead to forced liquidation. You need to re-adjust the transfer amount to make sure the position is at a safe level before proceeding with the transfer. |
| 58122      | 200              | A portion of your spot is being used for Delta offset between positions. If the transfer amount exceeds the available amount, it may affect current spot-derivatives risk offset structure, which will result in an increased Maintenance Margin Requirement (MMR) rate. Please be aware of your risk level. |
| 58123      | 200              | The From parameter cannot be the same as the To parameter.                                                                                     |
| 58124      | 200              | Your transfer is being processed, transfer id:{trId}. Please check the latest state of your transfer from the endpoint (GET /api/v5/asset/transfer-state) |
| 58125      | 200              | Non-tradable assets can only be transferred from sub-accounts to main accounts                                                                |
| 58126      | 200              | Non-tradable assets can only be transferred between funding accounts                                                                          |
| 58127      | 200              | Main account API key does not support current transfer 'type' parameter. Please refer to the API documentation.                                |
| 58128      | 200              | Sub-account API key does not support current transfer 'type' parameter. Please refer to the API documentation.                                 |
| 58129      | 200              | {param} is incorrect or {param} does not match with 'type'                                                                                   |
| 58131      | 200              | For compliance, we're unable to provide services to unverified users. Verify your identity to make a transfer.                                 |
| 58132      | 200              | For compliance, we're unable to provide services to users with Basic verification (Level 1). Complete Advanced verification (Level 2) to make a transfer. |
| 58200      | 200              | Withdrawal from {0} to {1} is currently not supported for this currency.                                                                     |
| 58201      | 200              | Withdrawal amount exceeds daily withdrawal limit.                                                                                            |
| 58202      | 200              | The minimum withdrawal amount for NEO is 1, and the amount must be an integer.                                                               |
| 58203      | 200              | Please add a withdrawal address.                                                                                                             |
| 58204      | 200              | Withdrawal suspended due to your account activity triggering risk control. Please contact customer support for assistance.                   |
| 58205      | 200              | Withdrawal amount exceeds the upper limit.                                                                                                  |
| 58206      | 200              | Withdrawal amount is less than the lower limit.                                                                                            |
| 58207      | 200              | Withdrawal address isn't on the verified address list. (The format for withdrawal addresses with a label is “address:label”.)               |
| 58208      | 200              | Withdrawal failed. Please link your email.                                                                                                  |
| 58209      | 200              | Sub-accounts don't support withdrawals or deposits. Please use your main account instead                                                     |
| 58210      | 200              | You can't proceed with withdrawal as we're unable to verify your identity. Please withdraw via our app or website instead.                 |
| 58212      | 200              | Withdrawal fee must be {0}% of the withdrawal amount                                                                                        |
| 58213      | 200              | The internal transfer address is illegal. It must be an email, phone number, or account name                                                 |
| 58214      | 200              | Withdrawals suspended due to {chainName} maintenance                                                                                        |
| 58215      | 200              | Withdrawal ID does not exist.                                                                                                               |
| 58216      | 200              | Operation not allowed.                                                                                                                      |
| 58217      | 200              | Withdrawals are temporarily suspended for your account due to a risk detected in your withdrawal address. Contact customer support for assistance |
| 58218      | 200              | The internal withdrawal failed. Please check the parameters toAddr and areaCode.                                                           |
| 58219      | 200              | You cannot withdraw crypto within 24 hours after changing your mobile number, email address, or Google Authenticator.                      |
| 58220      | 200              | Withdrawal request already canceled.                                                                                                       |
| 58221      | 200              | The toAddr parameter format is incorrect, withdrawal address needs labels. The format should be "address:label".                          |
| 58222      | 200              | Invalid withdrawal address                                                                                                                 |
| 58223      | 200              | This is a contract address with higher withdrawal fees                                                                                      |
| 58224      | 200              | This crypto currently doesn't support on-chain withdrawals to OKX addresses. Withdraw through internal transfers instead                   |
| 58225      | 200              | Asset transfers to unverified users in {region} are not supported due to local laws and regulations.                                        |
| 58226      | 200              | {chainName} is delisted and not available for crypto withdrawal.                                                                           |
| 58227      | 200              | Withdrawal of non-tradable assets can be withdrawn all at once only                                                                         |
| 58228      | 200              | Withdrawal of non-tradable assets requires that the API key must be bound to an IP                                                          |
| 58229      | 200              | Insufficient funding account balance to pay fees {fee} USDT                                                                                |
| 58230      | 200              | According to the OKX compliance policy, you will need to complete your identity verification (Level 1) in order to withdraw                  |
| 58231      | 200              | The recipient has not completed personal info verification (Level 1) and cannot receive your transfer                                       |
| 58232      | 200              | You’ve reached the personal information verification (L1) withdrawal limit, complete photo verification (L2) to increase the withdrawal limit |
| 58233      | 200              | For compliance, we're unable to provide services to unverified users. Verify your identity to withdraw.                                     |
| 58234      | 200              | For compliance, the recipient can't receive your transfer yet. They'll need to verify their identity to receive your transfer.               |
| 58235      | 200              | For compliance, we're unable to provide services to users with Basic verification (Level 1). Complete Advanced verification (Level 2) to withdraw. |
| 58236      | 200              | For compliance, a recipient with Basic verification (Level 1) is unable to receive your transfer. They'll need to complete Advanced verification (Level 2). |
| 58237      | 200              | According to local laws and regulations, please provide accurate recipient information (rcvrInfo). For the exchange address, please also provide exchange information and recipient identity information ({consientParameters}). |
| 58238      | 200              | Incomplete info. The info of the exchange and the recipient are required if you're withdrawing to an exchange platform.                      |
| 58239      | 200              | You can't withdraw to a private wallet via API. Please withdraw via our app or website instead.                                               |
| 58240      | 200              | For security and compliance purposes, please complete the identity verification process to use our services. If you prefer not to verify, contact customer support for next steps. We're committed to ensuring a safe platform for users and appreciate your understanding. |
| 58241      | 200              | Due to local compliance requirements, internal withdrawal is unavailable                                                                      |
| 58242      | 200              | The recipient can't receive your transfer due to their local compliance requirements                                                          |
| 58243      | 200              | Your recipient can't receive your transfer as they haven't made a cash deposit yet                                                             |
| 58244      | 200              | Make a cash deposit to proceed                                                                                                                |
| 58248      | 200              | Due to local regulations, API withdrawal isn't allowed. Withdraw using OKX app or web.                                                        |
| 58249      | 200              | API withdrawal for this currency is currently unavailable. Try withdrawing via our app or website.                                            |
| 58252      | 200              | Withdrawal is restricted for 48h after your first TRY transaction for asset security.                                                          |
| 58300      | 200              | Deposit-address count exceeds the limit.                                                                                                      |
| 58301      | 200              | Deposit-address not exist.                                                                                                                    |
| 58302      | 200              | Deposit-address needs tag.                                                                                                                    |
| 58303      | 200              | Deposit for chain {chain} is currently unavailable                                                                                            |
| 58304      | 200              | Failed to create invoice.                                                                                                                     |
| 58305      | 200              | Unable to retrieve deposit address, please complete identity verification and generate deposit address first.                                 |
| 58306      | 200              | According to the OKX compliance policy, you will need to complete your identity verification (Level 1) in order to deposit                   |
| 58307      | 200              | You've reached the personal information verification (L1) deposit limit, the excess amount has been frozen, complete photo verification (L2) to increase the deposit limit |
| 58308      | 200              | For compliance, we're unable to provide services to unverified users. Verify your identity to deposit.                                       |
| 58309      | 200              | For compliance, we're unable to provide services to users with Basic verification (Level 1). Complete Advanced verification (Level 2) to deposit. |
| 58310      | 200              | Unable to create new deposit address, try again later                                                                                        |
| 58350      | 200              | Insufficient balance.                                                                                                                        |
| 58351      | 200              | Invoice expired.                                                                                                                             |
| 58352      | 200              | Invalid invoice.                                                                                                                             |
| 58353      | 200              | Deposit amount must be within limits.                                                                                                       |
| 58354      | 200              | You have reached the daily limit of 10,000 invoices.                                                                                        |
| 58355      | 200              | Permission denied. Please contact your account manager.                                                                                      |
| 58356      | 200              | The accounts of the same node do not support the Lightning network deposit or withdrawal.                                                    |
| 58358      | 200              | The fromCcy parameter cannot be the same as the toCcy parameter.                                                                             |
| 58373      | 200              | The minimum {ccy} conversion amount is {amount}                                                                                             |
| 58400      | 200              | Request Failed                                                                                                                              |
| 58401      | 200              | Payment method is not supported                                                                                                             |
| 58402      | 200              | Invalid payment account                                                                                                                     |
| 58403      | 200              | Transaction cannot be canceled                                                                                                              |
| 58404      | 200              | ClientId already exists                                                                                                                     |
| 58405      | 200              | Withdrawal suspended                                                                                                                        |
| 58406      | 200              | Channel is not supported                                                                                                                    |
| 58407      | 200              | API withdrawal isn't allowed for this payment method. Withdraw using OKX app or web                                                        |

***
