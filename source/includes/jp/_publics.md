# 公開API

## サーバー時刻の照会

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

> 応答結果は下記のとおりです：

```json
{
  "status": 0,
  "data": 1523430502977
}
```

このAPIを使用して、サーバー時刻を取得できます。

### HTTP Request

`GET https://api.fcoin.com/v2/public/server-time`





## 取扱い仮想通貨の照会

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

> 応答結果は下記のとおりです：

```json
{
  "status": 0,
  "data": [
    "btc",
    "eth"
  ]
}
```

このAPIを使用して、取扱い仮想通貨を取得できます。

### HTTP Request

`GET https://api.fcoin.com/v2/public/currencies`





## 取引通貨ペアの照会

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

> 応答結果は下記のとおりです：

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

このAPIを使用して、取引通貨ペアを取得できます。

### HTTP Request

`GET https://api.fcoin.com/v2/public/symbols`
