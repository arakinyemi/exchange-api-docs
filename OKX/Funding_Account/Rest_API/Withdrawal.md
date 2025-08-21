### Withdrawal
Only supported withdrawal of assets from funding account. Common sub-account does not support withdrawal.

**Note:** The API can only make withdrawal to verified addresses/account, and verified addresses can be set by WEB/APP.

#### About tag
Some token deposits require a deposit address and a tag (e.g. Memo/Payment ID), which is a string that guarantees the uniqueness of your deposit address. Follow the deposit procedure carefully, or you may risk losing your assets.
For currencies with labels, if it is a withdrawal between OKX users, please use internal transfer instead of online withdrawal

#### API withdrawal restrictions for EEA users
Due to recent updates to the Travel Rule regulations within the EEA, API withdrawal to private wallet is not allowed and the user can continue to withdraw crypto through our website or app. If the user withdraw to the exchange wallet, relevant information about the recipient must be provided. More details please refer to https://eea.okx.com/docs-v5/log_en/#2025-01-14-withdrawal-api-adjustment-for-eea-entity-users

**Rate Limit:** 6 requests per second  
**Rate limit rule:** User ID  
**Permission:** Withdraw

HTTP Request
```
POST /api/v5/asset/withdrawal
```

Request Example
```bash
# Withdrawal to exchange wallet
POST /api/v5/asset/withdrawal
body
{
    "amt": "1000",
    "dest": "4",
    "ccy": "USDT",
    "toAddr": "TYW7C5qjhQtuhjz5qLQtA9bzaeEFo6ueg7",
    "chain": "USDT-TRC20",
    "rcvrInfo": {
        "walletType": "exchange",
        "exchId":"did:ethr:0x401c4bc0241bc787f64510f21a4ae7ec3603487c",
        "rcvrFirstName":"Bruce",
        "rcvrLastName":"Wayne",
        "rcvrCountry":"United States",
        "rcvrStreetName":"Clementi Avenue 1",
        "rcvrTownName":"San Jose",
        "rcvrCountrySubDivision":"California"
    }
}
```

Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| ccy | String | Yes | Currency, e.g. USDT |
| amt | String | Yes | Withdrawal amount<br>Withdrawal fee is not included in withdrawal amount. Please reserve sufficient transaction fees when withdrawing.<br>You can get fee amount by Get currencies.<br>For internal transfer, transaction fee is always 0. |
| dest | String | Yes | Withdrawal method<br>3: internal transfer<br>4: on-chain withdrawal |
| toAddr | String | Yes | toAddr should be a trusted address/account.<br>If your dest is 4, some crypto currency addresses are formatted as 'address:tag', e.g. 'ARDOR-7JF3-8F2E-QUWZ-CAN7F:123456'<br>If your dest is 3,toAddr should be a recipient address which can be UID, email, phone or login account name (account name is only for sub-account). |
| toAddrType | String | No | Address type<br>1: wallet address, email, phone, or login account name<br>2: UID (only applicable for dest=3) |
| chain | String | Conditional | Chain name<br>There are multiple chains under some currencies, such as USDT has USDT-ERC20, USDT-TRC20<br>If the parameter is not filled in, the default will be the main chain.<br>When you withdrawal the non-tradable asset, if the parameter is not filled in, the default will be the unique withdrawal chain.<br>Apply to on-chain withdrawal.<br>You can get supported chain name by the endpoint of Get currencies. |
| areaCode | String | Conditional | Area code for the phone number, e.g. 86<br>If toAddr is a phone number, this parameter is required.<br>Apply to internal transfer |
| rcvrInfo | Object | Conditional | Recipient information<br>For the specific entity users to do on-chain withdrawal/lightning withdrawal, this information is required. |
| > walletType | String | Yes | Wallet Type<br>exchange: Withdraw to exchange wallet<br>private: Withdraw to private wallet<br>For the wallet belongs to business recipient, rcvrFirstName may input the company name, rcvrLastName may input "N/A", location info may input the registered address of the company. |
| > exchId | String | Conditional | Exchange ID<br>You can query supported exchanges through the endpoint of Get exchange list (public)<br>If the exchange is not in the exchange list, fill in '0' in this field.<br>Apply to walletType = exchange |
| > rcvrFirstName | String | Conditional | Receiver's first name, e.g. Bruce |
| > rcvrLastName | String | Conditional | Receiver's last name, e.g. Wayne |
| > rcvrCountry | String | Conditional | The recipient's country, e.g. United States<br>You must enter an English country name or a two letter country code (ISO 3166-1). Please refer to the Country Name and Country Code in the country information table below. |
| > rcvrCountrySubDivision | String | Conditional | State/Province of the recipient, e.g. California |
| > rcvrTownName | String | Conditional | The town/city where the recipient is located, e.g. San Jose |

| > rcvrStreetName | String | Conditional |	Recipient's street address, e.g. Clementi Avenue 1 |
| clientId |	String |	No |	Client-supplied ID A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |


Response Example

```
{
    "code": "0",
    "msg": "",
    "data": [{
        "amt": "0.1",
        "wdId": "67485",
        "ccy": "BTC",
        "clientId": "",
        "chain": "BTC-Bitcoin"
    }]
}
```

Response Parameters

| Parameter  | Type   | Description |
|------------|--------|-------------|
| ccy        | String | Currency |
| chain      | String | Chain name, e.g. USDT-ERC20, USDT-TRC20 |
| amt        | String | Withdrawal amount |
| wdId       | String | Withdrawal ID |
| clientId   | String | Client-supplied ID. A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |

Country Information

| Country Name                  | Country Code |
|-------------------------------|--------------|
| Afghanistan                   | AF           |
| Albania                       | AL           |
| Algeria                       | DZ           |
| .....                         |              |
