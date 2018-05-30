# 订单

## 订单模型说明

订单模型由以下属性构成：

属性 | 类型 | 含义解释
---------- | ------- | -------
`id` | `String` | 订单 ID
`symbol` | `String` | 交易对
`side` | `String` | 交易方向（`buy`, `sell`）
`type` | `String` | 订单类型（`limit`，`market`）
`price` | `String` | 下单价格
`amount` | `String` | 下单数量
`state` | `String` | 订单状态
`executed_value` | `String` | 已成交
`filled_amount` | `String` | 成交量
`fill_fees` | `String` | 手续费
`created_at` | `Long` | 创建时间
`source` | `String` | 来源

订单状态说明：

属性  | 含义解释
----------- | -------
`submitted` | 已提交
`partial_filled` | 部分成交
`partial_canceled` | 部分成交已撤销
`filled` | 完全成交
`canceled` | 已撤销
`pending_cancel` | 撤销已提交

## 创建新的订单

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
order_create_param = fcoin.order_create_param('btcusdt', 'buy', 'limit', '8000.0', '1.0')
api.orders.create(order_create_param)
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
let orderCreateParam = fcoin.orderCreateParam('btcusdt', 'buy', 'limit', '8000.0', '1.0');
let orders = api.orders.create(orderCreateParam);
```

> 响应结果如下：

```json
{
  "status": 0,
  "data": "9d17a03b852e48c0b3920c7412867623"
}
```

此 API 用于创建新的订单。

### HTTP Request

`POST https://api.fcoin.com/v2/orders`

### 请求参数

参数 | 默认值 | 描述
--------- | ------- | -----------
symbol | 无  | 交易对
side | 无 | 交易方向
type | 无  | 订单类型
price | 无 | 价格
amount | 无 | 下单量






## 查询订单列表

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
api.orders.get()
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
let orders = api.orders.get();
```

> 响应结果如下：

```json
{
  "status": 0,
  "data": [
    {
      "id": "string",
      "symbol": "string",
      "type": "limit",
      "side": "buy",
      "price": "string",
      "amount": "string",
      "state": "submitted",
      "executed_value": "string",
      "fill_fees": "string",
      "filled_amount": "string",
      "created_at": 0,
      "source": "web"
    }
  ]
}
```

此 API 用于查询订单列表。

### HTTP Request

`GET https://api.fcoin.com/v2/orders`

### 查询参数

参数 | 默认值 | 描述
--------- | ------- | -----------
symbol |  | 交易对
states |  | 订单状态
before |  | 查询某个页码之前的订单
after |  | 查询某个页码之后的订单
limit |  | 每页的订单数量，默认为 20 条





## 获取指定订单

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
api.orders.get('9d17a03b852e48c0b3920c7412867623')
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
let max = api.orders.get('9d17a03b852e48c0b3920c7412867623');
```

> 响应结果如下：

```json
{
  "status": 0,
  "data": {
    "id": "9d17a03b852e48c0b3920c7412867623",
    "symbol": "string",
    "type": "limit",
    "side": "buy",
    "price": "string",
    "amount": "string",
    "state": "submitted",
    "executed_value": "string",
    "fill_fees": "string",
    "filled_amount": "string",
    "created_at": 0,
    "source": "web"
  }
}
```

此 API 用于返回指定的订单详情。

### HTTP Request

`GET https://api.fcoin.com/v2/orders/{order_id}`

### URL 参数

参数 | 描述
--------- | -----------
order_id | 订单 ID






## 申请撤销订单

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
api.orders.submit_cancel(2)
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
let max = api.orders.submitCancel(2);
```

> 响应结果如下：

```json
{
  "status": 0,
  "msg": "string",
  "data": true
}
```

此 API 用于撤销指定订单，订单撤销过程是异步的，即此 API 的调用成功代表着订单已经进入撤销申请的过程，需要等待撮合的进一步处理，才能进行订单的撤销确认。

### HTTP Request

`POST https://api.fcoin.com/v2/orders/{order_id}/submit-cancel`

### URL 参数

参数 | 解释
--------- | -----------
order_id | 订单 ID






## 查询指定订单的成交记录

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
api.orders.get('9d17a03b852e48c0b3920c7412867623').match_results()
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
let max = api.orders.get('9d17a03b852e48c0b3920c7412867623').matchResults();
```

> 响应结果如下：

```json
{
  "status": 0,
  "data": [
    {
      "price": "string",
      "fill_fees": "string",
      "filled_amount": "string",
      "side": "buy",
      "type": "limit",
      "created_at": 0
    }
  ]
}
```

此 API 用于获取指定订单的成交记录

### HTTP Request

`GET https://api.fcoin.com/v2/orders/{order_id}/match-results`

### URL 参数

参数 | 解释
--------- | -----------
order_id | 订单 ID
