# Fetch withdraw quota 

## API Description​

Fetch withdraw quota

## HTTP Request​

GET `/sapi/v1/capital/withdraw/quota`

## Request Weight(IP)​

**10**

## Request Parameters​

NONE

## Response Example​

```json
{  
    "wdQuota" : "10000", //User's total withdrawal quota in the past 24 hours (including on-chain withdrawal and internal transfer), unit in USD  
    "usedWdQuota" : "1000" //User withdrawal quota usage in the past 24 hours, unit in USD  
}
```

