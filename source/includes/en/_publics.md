# Public interface

## Query server time

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
server_time = api.server_time()
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
let serverTime = api.serverTime();
```

> The response results are as follows：

```json
{
  "status": 0,
  "data": 1523430502977
}
```

The API is used to obtain the server time。

### HTTP Request

`GET https://api.fcoin.com/v2/public/server-time`





## Query available currencies

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
currencies = api.currencies()
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
let currencies = api.currencies();
```

> The response results are as follows：

```json
{
  "status": 0,
  "data": [
    "btc",
    "eth"
  ]
}
```

This API is used to obtain the available currencies。

### HTTP Request

`GET https://api.fcoin.com/v2/public/currencies`





## Query available trading pairs

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
symbols = api.symbols()
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
let symbols = api.symbols();
```

> The response results are as follows：

```json
{
  "status": 0,
  "data": [
    {
      "name": "btcusdt",
      "base_currency": "btc",
      "quote_currency": "usdt",
      "price_decimal": 2,
      "amount_decimal": 4
    },
    {
      "name": "ethusdt",
      "base_currency": "eth",
      "quote_currency": "usdt",
      "price_decimal": 2,
      "amount_decimal": 4
    }
  ]
}
```

This API is used to get available trading pairs。

### HTTP Request

`GET https://api.fcoin.com/v2/public/symbols`
