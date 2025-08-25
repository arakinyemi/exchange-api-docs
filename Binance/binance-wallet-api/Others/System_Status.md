# System Status 

## API Description​

Fetch system status.

## HTTP Request​

GET `/sapi/v1/system/status`

## Request Weight(IP)​

**1**

## Response Example​

```json
{   
    "status": 0,              // 0: normal，1：system maintenance  
    "msg": "normal"           // "normal", "system_maintenance"  
}
```


