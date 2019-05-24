# 公开接口

## 查询服务器时间

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

> 响应结果如下：

```json
{
  "status": 0,
  "data": 1523430502977
}
```

此 API 用于获取服务器时间。

### HTTP Request

`GET https://api.fcoin.com/v2/public/server-time`





## 查询可用币种

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

> 响应结果如下：

```json
{
  "status": 0,
  "data": [
    "btc",
    "eth"
  ]
}
```

此 API 用于获取可用币种。

### HTTP Request

`GET https://api.fcoin.com/v2/public/currencies`





## 查询可用交易对

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

> 响应结果如下：

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

此 API 用于获取可用交易对。

### HTTP Request

`GET https://api.fcoin.com/v2/public/symbols`

或者，推荐使用下面接口
`GET https://www.fcoin.com/openapi/v2/symbols`