# Request Security

Every method has a security type which determines how to call it.  
Security type is stated next to the method name. For example, **Place new order .**  
If no security type is stated, the security type is **NONE**.

## Security Types

| Security type | API key  | Signature | Description                                                        |
|---------------|----------|-----------|--------------------------------------------------------------------|
| **NONE**      |          |           | Public market data                                                 |
| **TRADE**     | required | required  | Trading on the exchange, placing and canceling orders              |
| **USER_DATA** | required | required  | Private account information, such as order status and your trading history |
| **USER_STREAM** | required |           | Managing User Data Stream subscriptions                            |

---

## Secure Methods

Secure methods require a valid API key to be specified and authenticated.  
API keys can be created on the **API Management** page of your Binance account.  

Both API key and secret key are sensitive. **Never share them with anyone.**  
If you notice unusual activity in your account, immediately revoke all the keys and contact Binance support.

---

## API Key Configuration

API keys can be configured to allow access only to certain types of secure methods.  
For example, you can have an API key with **TRADE** permission for trading, while using a separate API key with **USER_DATA** permission to monitor your order status.

By default, an API key cannot **TRADE**. You need to enable trading in API Management first.

---

## SIGNED Requests

TRADE and USER_DATA requests are also known as **SIGNED requests**.

### SIGNED (TRADE and USER\_DATA) request security​

* `SIGNED` requests require an additional parameter: `signature`, authorizing the request.
* Please consult SIGNED request example (HMAC), SIGNED request example (RSA), and SIGNED request example (Ed25519) on how to compute signature, depending on which API key type you are using.

### Timing security​

* `SIGNED` requests also require a `timestamp` parameter which should be the current timestamp either in milliseconds or microseconds. (See General API Information)
* An additional optional parameter, `recvWindow`, specifies for how long the request stays valid and may only be specified in milliseconds.
  * If `recvWindow` is not sent, **it defaults to 5000 milliseconds**.
  * Maximum `recvWindow` is 60000 milliseconds.
* Request processing logic is as follows:

```
serverTime = getCurrentTime()  
if (timestamp < (serverTime + 1 second) && (serverTime - timestamp) <= recvWindow) {  
  // begin processing request  
  serverTime = getCurrentTime()  
  if (serverTime - timestamp) <= recvWindow {  
    // forward request to Matching Engine  
  } else {  
    // reject request  
  }  
  // finish processing request  
} else {  
  // reject request  
}
```

**Serious trading is about timing.** Networks can be unstable and unreliable,
which can lead to requests taking varying amounts of time to reach the
servers. With `recvWindow`, you can specify that the request must be
processed within a certain number of milliseconds or be rejected by the
server.

**It is recommended to use a small `recvWindow` of 5000 or less!**


### SIGNED request example (HMAC)​

Here is a step-by-step guide on how to sign requests using HMAC secret key.

Example API key and secret key:

| Key | Value |
| --- | --- |
| apiKey | `vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A` |
| secretKey | `NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j` |

**WARNING: DO NOT SHARE YOUR API KEY AND SECRET KEY WITH ANYONE.**

The example keys are provided here only for illustrative purposes.

Example of request:

```json
{  
  "id": "4885f793-e5ad-4c3b-8f6c-55d891472b71",  
  "method": "order.place",  
  "params": {  
    "symbol":           "BTCUSDT",  
    "side":             "SELL",  
    "type":             "LIMIT",  
    "timeInForce":      "GTC",  
    "quantity":         "0.01000000",  
    "price":            "52000.00",  
    "newOrderRespType": "ACK",  
    "recvWindow":       100,  
    "timestamp":        1645423376532,  
    "apiKey":           "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature":        "------ FILL ME ------"  
  }  
}
```

As you can see, the `signature` parameter is currently missing.

**Step 1. Construct the signature payload**

Take all request `params` except for the `signature`, sort them by name in alphabetical order:

| Parameter | Value |
| --- | --- |
| apiKey | vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A |
| newOrderRespType | ACK |
| price | 52000.00 |
| quantity | 0.01000000 |
| recvWindow | 100 |
| side | SELL |
| symbol | BTCUSDT |
| timeInForce | GTC |
| timestamp | 1645423376532 |
| type | LIMIT |

Format parameters as `parameter=value` pairs separated by `&`.

Resulting signature payload:

```
apiKey=vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A&newOrderRespType=ACK&price=52000.00&quantity=0.01000000&recvWindow=100&side=SELL&symbol=BTCUSDT&timeInForce=GTC&timestamp=1645423376532&type=LIMIT
```

**Step 2. Compute the signature**

1. Interpret `secretKey` as ASCII data, using it as a key for HMAC-SHA-256.
2. Sign signature payload as ASCII data.
3. Encode HMAC-SHA-256 output as a hex string.

Note that `apiKey`, `secretKey`, and the payload are **case-sensitive**, while resulting signature value is case-insensitive.

You can cross-check your signature algorithm implementation with OpenSSL:

```
$ echo -n 'apiKey=vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A&newOrderRespType=ACK&price=52000.00&quantity=0.01000000&recvWindow=100&side=SELL&symbol=BTCUSDT&timeInForce=GTC&timestamp=1645423376532&type=LIMIT' \  
  | openssl dgst -hex -sha256 -hmac 'NhqPtmdSJYdKjVHjA7PZj4Mge3R5YNiP1e3UZjInClVN65XAbvqqM6A7H5fATj0j'  
  
cc15477742bd704c29492d96c7ead9414dfd8e0ec4a00f947bb5bb454ddbd08a
```

**Step 3. Add `signature` to request `params`**

Finally, complete the request by adding the `signature` parameter with the signature string.

```json
{  
  "id": "4885f793-e5ad-4c3b-8f6c-55d891472b71",  
  "method": "order.place",  
  "params": {  
    "symbol":           "BTCUSDT",  
    "side":             "SELL",  
    "type":             "LIMIT",  
    "timeInForce":      "GTC",  
    "quantity":         "0.01000000",  
    "price":            "52000.00",  
    "newOrderRespType": "ACK",  
    "recvWindow":       100,  
    "timestamp":        1645423376532,  
    "apiKey":           "vmPUZE6mv9SD5VNHk4HlWFsOr6aKE2zvsw0MuIgwCIPy6utIco14y7Ju91duEh8A",  
    "signature":        "cc15477742bd704c29492d96c7ead9414dfd8e0ec4a00f947bb5bb454ddbd08a"  
  }  
}
```

### SIGNED request example (RSA)​

Here is a step-by-step guide on how to sign requests using your RSA private key.

| Key | Value |
| --- | --- |
| apiKey | `CAvIjXy3F44yW6Pou5k8Dy1swsYDWJZLeoK2r8G4cFDnE9nosRppc2eKc1T8TRTQ` |

In this example, we assume the private key is stored in the `test-prv-key.pem` file.

**WARNING: DO NOT SHARE YOUR API KEY AND PRIVATE KEY WITH ANYONE.**

The example keys are provided here only for illustrative purposes.

Example of request:

```json
{  
  "id": "4885f793-e5ad-4c3b-8f6c-55d891472b71",  
  "method": "order.place",  
  "params": {  
    "symbol":           "BTCUSDT",  
    "side":             "SELL",  
    "type":             "LIMIT",  
    "timeInForce":      "GTC",  
    "quantity":         "0.01000000",  
    "price":            "52000.00",  
    "newOrderRespType": "ACK",  
    "recvWindow":       100,  
    "timestamp":        1645423376532,  
    "apiKey":           "CAvIjXy3F44yW6Pou5k8Dy1swsYDWJZLeoK2r8G4cFDnE9nosRppc2eKc1T8TRTQ",  
    "signature":        "------ FILL ME ------"  
  }  
}
```

**Step 1. Construct the signature payload**

Take all request `params` except for the `signature`, sort them by name in alphabetical order:

| Parameter | Value |
| --- | --- |
| apiKey | CAvIjXy3F44yW6Pou5k8Dy1swsYDWJZLeoK2r8G4cFDnE9nosRppc2eKc1T8TRTQ |
| newOrderRespType | ACK |
| price | 52000.00 |
| quantity | 0.01000000 |
| recvWindow | 100 |
| side | SELL |
| symbol | BTCUSDT |
| timeInForce | GTC |
| timestamp | 1645423376532 |
| type | LIMIT |

Format parameters as `parameter=value` pairs separated by `&`.

Resulting signature payload:

```
apiKey=CAvIjXy3F44yW6Pou5k8Dy1swsYDWJZLeoK2r8G4cFDnE9nosRppc2eKc1T8TRTQ&newOrderRespType=ACK&price=52000.00&quantity=0.01000000&recvWindow=100&side=SELL&symbol=BTCUSDT&timeInForce=GTC&timestamp=1645423376532&type=LIMIT
```

**Step 2. Compute the signature**

1. Encode signature payload as ASCII data.
2. Sign payload using RSASSA-PKCS1-v1\_5 algorithm with SHA-256 hash function.
3. Encode output as base64 string.

Note that `apiKey`, the payload, and the resulting `signature` are **case-sensitive**.

You can cross-check your signature algorithm implementation with OpenSSL:

```
$ echo -n 'apiKey=CAvIjXy3F44yW6Pou5k8Dy1swsYDWJZLeoK2r8G4cFDnE9nosRppc2eKc1T8TRTQ&newOrderRespType=ACK&price=52000.00&quantity=0.01000000&recvWindow=100&side=SELL&symbol=BTCUSDT&timeInForce=GTC&timestamp=1645423376532&type=LIMIT' \  
  | openssl dgst -sha256 -sign test-prv-key.pem \  
  | openssl enc -base64 -A  
  
OJJaf8C/3VGrU4ATTR4GiUDqL2FboSE1Qw7UnnoYNfXTXHubIl1iaePGuGyfct4NPu5oVEZCH4Q6ZStfB1w4ssgu0uiB/Bg+fBrRFfVgVaLKBdYHMvT+ljUJzqVaeoThG9oXlduiw8PbS9U8DYAbDvWN3jqZLo4Z2YJbyovyDAvDTr/oC0+vssLqP7NmlNb3fF3Bj7StmOwJvQJTbRAtzxK5PP7OQe+0mbW+D7RqVkUiSswR8qJFWTeSe4nXXNIdZdueYhF/Xf25L+KitJS5IHdIHcKfEw3MQzHFb2ZsGWkjDQwxkwr7Noi0Zaa+gFtxCuatGFm9dFIyx217pmSHtA==
```

**Step 3. Add `signature` to request `params`**

Finally, complete the request by adding the `signature` parameter with the signature string.

```json
{  
  "id": "4885f793-e5ad-4c3b-8f6c-55d891472b71",  
  "method": "order.place",  
  "params": {  
    "symbol":           "BTCUSDT",  
    "side":             "SELL",  
    "type":             "LIMIT",  
    "timeInForce":      "GTC",  
    "quantity":         "0.01000000",  
    "price":            "52000.00",  
    "newOrderRespType": "ACK",  
    "recvWindow":       100,  
    "timestamp":        1645423376532,  
    "apiKey":           "CAvIjXy3F44yW6Pou5k8Dy1swsYDWJZLeoK2r8G4cFDnE9nosRppc2eKc1T8TRTQ",  
    "signature":        "OJJaf8C/3VGrU4ATTR4GiUDqL2FboSE1Qw7UnnoYNfXTXHubIl1iaePGuGyfct4NPu5oVEZCH4Q6ZStfB1w4ssgu0uiB/Bg+fBrRFfVgVaLKBdYHMvT+ljUJzqVaeoThG9oXlduiw8PbS9U8DYAbDvWN3jqZLo4Z2YJbyovyDAvDTr/oC0+vssLqP7NmlNb3fF3Bj7StmOwJvQJTbRAtzxK5PP7OQe+0mbW+D7RqVkUiSswR8qJFWTeSe4nXXNIdZdueYhF/Xf25L+KitJS5IHdIHcKfEw3MQzHFb2ZsGWkjDQwxkwr7Noi0Zaa+gFtxCuatGFm9dFIyx217pmSHtA=="  
  }  
}
```


### SIGNED Request Example (Ed25519)​

**Note: It is highly recommended to use Ed25519 API keys as it should provide the best performance and security out of all supported key types.**

| Parameter | Value |
| --- | --- |
| `symbol` | BTCUSDT |
| `side` | SELL |
| `type` | LIMIT |
| `timeInForce` | GTC |
| `quantity` | 1 |
| `price` | 0.2 |
| `timestamp` | 1668481559918 |

This is a sample code in Python to show how to sign the payload with an Ed25519 key.

```py
#!/usr/bin/env python3  
  
import base64  
import time  
import json  
from cryptography.hazmat.primitives.serialization import load_pem_private_key  
from websocket import create_connection  
  
# Set up authentication  
API_KEY='put your own API Key here'  
PRIVATE_KEY_PATH='test-prv-key.pem'  
  
# Load the private key.  
# In this example the key is expected to be stored without encryption,  
# but we recommend using a strong password for improved security.  
with open(PRIVATE_KEY_PATH, 'rb') as f:  
    private_key = load_pem_private_key(data=f.read(),  
                                       password=None)  
  
# Set up the request parameters  
params = {  
    'apiKey':        API_KEY,  
    'symbol':       'BTCUSDT',  
    'side':         'SELL',  
    'type':         'LIMIT',  
    'timeInForce':  'GTC',  
    'quantity':     '1.0000000',  
    'price':        '0.20'  
}  
  
# Timestamp the request  
timestamp = int(time.time() * 1000) # UNIX timestamp in milliseconds  
params['timestamp'] = timestamp  
  
# Sign the request  
payload = '&'.join([f'{param}={value}' for param, value in sorted(params.items())])  
  
signature = base64.b64encode(private_key.sign(payload.encode('ASCII')))  
params['signature'] = signature.decode('ASCII')  
  
# Send the request  
request = {  
    'id': 'my_new_order',  
    'method': 'order.place',  
    'params': params  
}  
  
ws = create_connection("wss://ws-api.binance.com:443/ws-api/v3")  
ws.send(json.dumps(request))  
result =  ws.recv()  
ws.close()  
  
print(result)
```

