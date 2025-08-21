### Overview

#### Making Requests

All private REST requests must contain the following headers:

OK-ACCESS-KEY The API key as a String.

OK-ACCESS-SIGN The Base64-encoded signature (see Signing Messages subsection for details).

OK-ACCESS-TIMESTAMP The UTC timestamp of your request .e.g : 2020-12-08T09:08:57.715Z

OK-ACCESS-PASSPHRASE The passphrase you specified when creating the API key.

Request bodies should have content type application/json and be in valid JSON format.

#### Signature

The OK-ACCESS-SIGN header is generated as follows:

Create a pre-hash string of timestamp + method + requestPath + body (where + represents String concatenation).
Prepare the SecretKey.
Sign the pre-hash string with the SecretKey using the HMAC SHA256.
Encode the signature in the Base64 format.
Example: sign=CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(timestamp + 'GET' + '/api/v5/account/balance?ccy=BTC', SecretKey))

The timestamp value is the same as the OK-ACCESS-TIMESTAMP header with millisecond ISO format, e.g. 2020-12-08T09:08:57.715Z.

The request method should be in UPPERCASE: e.g. GET and POST.

The requestPath is the path of requesting an endpoint.

Example: /api/v5/account/balance

The body refers to the String of the request body. It can be omitted if there is no request body (frequently the case for GET requests).

Example: {"instId":"BTC-USDT","lever":"5","mgnMode":"isolated"}

 `GET` request parameters are counted as requestpath, not body
The SecretKey is generated when you create an API key.

Example: 22582BD0CFF14C41EDBF1AB98506286D
