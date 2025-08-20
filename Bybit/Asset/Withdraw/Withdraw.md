# Withdraw
Withdraw assets from your Bybit account. You can make an off-chain transfer if the target wallet address is from Bybit. This means that no blockchain fee will be charged.


tip

Make sure you have whitelisted your wallet address here

Request by the master UID's api key only

formula

feeType=0:

withdrawPercentageFee != 0: handlingFee = inputAmount / (1 - withdrawPercentageFee) * withdrawPercentageFee + withdrawFee

withdrawPercentageFee = 0: handlingFee = withdrawFee

feeType=1:

withdrawPercentageFee != 0: handlingFee = withdrawFee + (inputAmount - withdrawFee) * withdrawPercentageFee

withdrawPercentageFee = 0: handlingFee = withdrawFee


HTTP Request
```http
POST /v5/asset/withdraw/create
```

Request Parameters
| Parameter                       | Required | Type    | Comments                                                                                                                                                                                                                                                                                                                                                                       |
|---------------------------------|----------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| coin                            | true     | string  | Coin, uppercase only                                                                                                                                                                                                                                                                                                                                                           |
| chain                           | false    | string  | Chain<br>forceChain=0 or 1: this field is required<br>forceChain=2: this field can be null                                                                                                                                                                                                                                                                                     |
| address                         | true     | string  | forceChain=0 or 1: fill wallet address, and make sure you add address in the address book first. Please note that the address is case sensitive, so use the exact same address added in address book<br>forceChain=2: fill Bybit UID, and it can only be another Bybit main account UID. Make sure you add UID in the address book first                                       |
| tag                             | false    | string  | Tag<br>Required if tag exists in the wallet address list.<br>Note: please do not set a tag/memo in the address book if the chain does not support tag                                                                                                                                                                                                                          |
| amount                          | true     | string  | Withdraw amount                                                                                                                                                                                                                                                                                                                                                                |
| timestamp                       | true     | integer | Current timestamp (ms). Used for preventing from withdraw replay                                                                                                                                                                                                                                                                                                               |
| forceChain                      | false    | integer | Whether or not to force an on-chain withdrawal<br>0(default): If the address is parsed out to be an internal address, then internal transfer (Bybit main account only)<br>1: Force the withdrawal to occur on-chain<br>2: Use UID to withdraw                                                                                                                                  |
| accountType                     | true     | string  | Select the wallet to be withdrawn from<br>FUND: Funding wallet<br>UTA: System transfers the funds to Funding wallet to withdraw<br>FUND,UTA: For combo withdrawals, funds will be deducted from the Funding wallet first. If the balance is insufficient, the remaining amount will be deducted from the UTA wallet.<br>SPOT: withdraw from spot wallet (classic account only) |
| feeType                         | false    | integer | Handling fee option<br>0(default): input amount is the actual amount received, so you have to calculate handling fee manually<br>1: input amount is not the actual amount you received, the system will help to deduct the handling fee automatically                                                                                                                          |
| requestId                       | false    | string  | Customised ID, globally unique, it is used for idempotent verification<br>A combination of letters (case sensitive) and numbers, which can be pure letters or pure numbers and the length must be between 1 and 32 digits                                                                                                                                                      |
| beneficiary                     | false    | Object  | Travel rule info. It is required for kyc/kyb=KOR (Korean), kyc=IND (India) users, and users who registered in Bybit Turkey(TR), Bybit Kazakhstan(KZ), Bybit Indonesia (ID)                                                                                                                                                                                                     |
| > vaspEntityId                  | false    | string  | Receiver exchange entity Id. Please call this endpoint to get this ID.<br>Required param for Korean users<br>Ignored by TR, KZ users                                                                                                                                                                                                                                           |
| > beneficiaryName               | false    | string  | Receiver exchange user KYC name<br>Rules for Korean users:<br>Please refer to target exchange kyc name<br>When vaspEntityId="others", this field can be nullRules for TR, KZ, kyc=IND users: it is a required param, fill with individual name or company name                                                                                                                 |
| > beneficiaryLegalType          | false    | string  | Beneficiary legal type, individual(default), company<br>Required param for TR, KZ, kyc=IND users<br>Korean users can ignore                                                                                                                                                                                                                                                    |
| > beneficiaryWalletType         | false    | string  | Beneficiary wallet type, 0: custodial/exchange wallet (default), 1: non custodial/exchane wallet<br>Required param for TR, KZ, kyc=IND users<br>Korean users can ignore                                                                                                                                                                                                        |
| > beneficiaryUnhostedWalletType | false    | string  | Beneficiary unhosted wallet type, 0: Your own wallet, 1: others' wallet<br>Required param for TR, KZ, kyc=IND users when "beneficiaryWalletType=1"<br>Korean users can ignore                                                                                                                                                                                                  |
| > beneficiaryPoiNumber          | false    | string  | Beneficiary ducument number<br>Required param for TR, KZ users<br>Korean users can ignore                                                                                                                                                                                                                                                                                      |
| > beneficiaryPoiType            | false    | string  | Beneficiary ducument type<br>Required param for TR, KZ users: ID card, Passport, driver license, residence permit, Business ID, etc<br>Korean users can ignore                                                                                                                                                                                                                 |
| > beneficiaryPoiIssuingCountry  | false    | string  | Beneficiary ducument issuing country<br>Required param for TR, KZ users: refer to Alpha-3 country code<br>Korean users can ignore                                                                                                                                                                                                                                              |
| > beneficiaryPoiExpiredDate     | false    | string  | Beneficiary ducument expiry date<br>Required param for TR, KZ users: yyyy-mm-dd format, e.g., "1990-02-15"<br>Korean users can ignore                                                                                                                                                                                                                                          |
| > beneficiaryAddressCountry     | false    | string  | Beneficiary country<br>Required param for UAE users only, e.g.,IDN                                                                                                                                                                                                                                                                                                             |
| > beneficiaryAddressState       | false    | string  | Beneficiary state<br>Required param for UAE users only, e.g., "ABC"                                                                                                                                                                                                                                                                                                            |
| > beneficiaryAddressCity        | false    | string  | Beneficiary city<br>Required param for UAE users only, e.g., "Jakarta"                                                                                                                                                                                                                                                                                                         |
| > beneficiaryAddressBuilding    | false    | string  | Beneficiary building address<br>Required param for UAE users only                                                                                                                                                                                                                                                                                                              |
| > beneficiaryAddressStreet      | false    | string  | Beneficiary street address<br>Required param for UAE users only                                                                                                                                                                                                                                                                                                                |
| > beneficiaryAddressPostalCode  | false    | string  | Beneficiary address post code<br>Required param for UAE users only                                                                                                                                                                                                                                                                                                             |
| > beneficiaryDateOfBirth        | false    | string  | Beneficiary date of birth<br>Required param for UAE users only                                                                                                                                                                                                                                                                                                                 |
| > beneficiaryPlaceOfBirth       | false    | string  | Beneficiary birth place<br>Required param for UAE users onl                                                                                                                                                                                                                                                                                                                    |



Response Parameters
| Parameter | Type | Comments |
| --------- | ---- | -------- |
| id | string | Withdrawal ID |

---

Request Example

HTTP
 
  
```http
POST /v5/asset/withdraw/create HTTP/1.1
Host: api-testnet.bybit.com
X-BAPI-API-KEY: xxxxxxxxxxxxxxxxxx
X-BAPI-TIMESTAMP: 1672196570254
X-BAPI-RECV-WINDOW: 5000
X-BAPI-SIGN: XXXXX
Content-Type: application/json
```

```json
{
    "coin": "USDT",
    "chain": "ETH",
    "address": "0x99ced129603abc771c0dabe935c326ff6c86645d",
    "amount": "24",
    "timestamp": 1672196561407,
    "forceChain": 0,
    "accountType": "FUND"
}
```

Response Example
```json
{
    "retCode": 0,
    "retMsg": "success",
    "result": {
        "id": "10195"
    },
    "retExtinfo
": {},
    "time": 1672196571239
}
```

