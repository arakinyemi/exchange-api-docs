### Overview

Currently, the API works for Demo Trading, but some functions are not supported, such as withdraw,deposit,purchase/redemption, etc.

The Demo Trading URL:

REST: https://eea.okx.com

Public WebSocket: wss://wseeapap.okx.com:8443/ws/v5/public

Private WebSocket: wss://wseeapap.okx.com:8443/ws/v5/private

Business WebSocket: wss://wseeapap.okx.com:8443/ws/v5/business

OKX account can be used for login on Demo Trading. If you already have an OKX account, you can log in directly.

Start API Demo Trading by the following steps:

Login OKX —> Trade —> Demo Trading —> Personal Center —> Demo Trading API -> Create Demo Trading API Key —> Start your Demo Trading

 Note: `x-simulated-trading: 1` needs to be added to the header of the Demo Trading request.
Http Header Example
```
Content-Type: application/json

OK-ACCESS-KEY: 37c541a1-****-****-****-10fe7a038418

OK-ACCESS-SIGN: leaVRETrtaoEQ3yI9qEtI1CZ82ikZ4xSG5Kj8gnl3uw=

OK-ACCESS-PASSPHRASE: 1****6

OK-ACCESS-TIMESTAMP: 2020-03-28T12:21:41.274Z

x-simulated-trading: 1
```
