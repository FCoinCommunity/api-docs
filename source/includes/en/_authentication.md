# Certification

> Execute the following code for user identification：

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
```

FCoin uses API key and API secret for identification，Please visit the settings center and register as a developer to obtain API key and API secret.

In  addition to the public API, API key and signature are also included in FCoin's API request




## Access restrictions

The current access frequency is 100 times / 10 seconds per user, and the access frequency limit will be differentiated according to the service in the future.




## API signature

The data prepared before signing is as follows：

`HTTP_METHOD` + `HTTP_REQUEST_URI` + `TIMESTAMP` + `POST_BODY`

After the connection is established, Base64 encoding is performed. The encoded data is HMAC-SHA1 signed, and the signature is subjected to secondary Base64 encoding. The various parts are explained as follows：

<aside class="warning">
Please note that twice `Base64` encoding is required!
</aside>

### HTTP_METHOD

`GET`, `POST`, `DELETE`, `PUT` requires capitalization

### HTTP_REQUEST_URI

`https://api.fcoin.com/v2/` is the prefix for v2 API request

Add the resource path you really want to access, such as `orders?param1=value1`, and finally `https://api.fcoin.com/v2/orders?param1=value1`

The parameters in the requested URI need to be sorted by alphabetically.

That is, if the requested URI is `https://api.fcoin.com/v2/orders?c=value1&b=value2&a=value3`, then the request parameters should be sorted alphabetically before signing, and the final signed URI is `https://api.fcoin.com/v2/orders?a=value3&b=value2&c=value1`. Please note that the order of the three parameters in the original request URI is `c, b, a`, and then `a, b, c` after ordering.

### TIMESTAMP

When accessing the API, the time difference between the UNIX EPOCH timestamp and the server should be no more than 30 seconds.

### POST_BODY

If it is a POST request, the POST request data also needs to be signed：

All the requested keys are sorted in alphabetical order, followed by url parameterization, and connected with `&`.

<aside class="warning">
Please note that the key values of POST_BODY need to be sorted alphabetically!
</aside>

> If the requested data is：

```json
{
  "username": "username",
  "password": "passowrd"
}
```

> then sort the key alphabetically and parameterize the url, ie：

```
password=password
username=username
```

> Because `p` is sorted before `u` in the alphabet, `password` is placed before `username` and then connected with `&`, ie:

```
password=password&username=username
```

## Full example

> For the following requests：

```
POST https://api.fcoin.com/v2/orders

{
  "type": "limit",
  "side": "buy",
  "amount": "100.0",
  "price": "100.0",
  "symbol": "btcusdt"
}

timestamp: 1523069544359
```

> The preparated data before signing is as follows：

```
POSThttps://api.fcoin.com/v2/orders1523069544359amount=100.0&price=100.0&side=buy&symbol=btcusdt&type=limit
```

> Perform Base64 encoding to obtain：

```
UE9TVGh0dHBzOi8vYXBpLmZjb2luLmNvbS92Mi9vcmRlcnMxNTIzMDY5NTQ0MzU5YW1vdW50PTEwMC4wJnByaWNlPTEwMC4wJnNpZGU9YnV5JnN5bWJvbD1idGN1c2R0JnR5cGU9bGltaXQ=
```

> Copy the key obtained when applying for the API Key (API SECRET). The following signature result is taken `3600d0a74aa3410fb3b1996cca2419c8` as an example，

> The obtained result is `HMAC-SHA1` signed with the secret key, and the binary result is `Base64` encoded, to obtain：

```
DeP6oftldIrys06uq3B7Lkh3a0U=
```

> That is, the final signature is generated for verification to the API server

## Parameter name

* `FC-ACCESS-KEY`
* `FC-ACCESS-SIGNATURE`
* `FC-ACCESS-TIMESTAMP`

## Description

You can use the developer tools (not yet open) for online joint debugging testing
