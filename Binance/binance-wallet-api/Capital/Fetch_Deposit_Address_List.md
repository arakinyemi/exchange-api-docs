# Fetch deposit address list with network

## API Description​

Fetch deposit address list with network.

## HTTP Request​

GET `/sapi/v1/capital/deposit/address/list`

## Request Weight(IP)​

**10**

## Request Parameters​

| Name | Type | Mandatory | Description |
| --- | --- | --- | --- |
| coin | STRING | YES | `coin` refers to the parent network address format that the address is using |
| network | STRING | NO |  |
| timestamp | LONG | YES |  |

> * If network is not send, return with default network of the coin.
> * You can get network and isDefault in networkList in the response of `Get /sapi/v1/capital/config/getall`.

## Response Example​

```json
[  
  {  
    "coin": "ETH", //coin here means network address space, ETH for all EVM-like network  
    "address": "0xD316E95Fd9E8E237Cb11f8200Babbc5D8D177BA4",  
    "tag":"",  
    "isDefault": 0  
  },   
  {  
    "coin": "ETH",  
    "address": "0xD316E95Fd9E8E237Cb11f8200Babbc5D8D177BA4",  
    "tag":"",  
    "isDefault": 0  
  },   
  {  
    "coin": "ETH",  
    "address": "0x00003ada75e7da97ba0db2fcde72131f712455e2",  
    "tag":"",  
    "isDefault": 1  //'isDefault' is 1 means the address is default, same as shown in the app.  
  }  
]
```

