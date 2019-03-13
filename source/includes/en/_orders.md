# Orders

## Description of the order model

The order model consists of the following properties：

Property | type | interpretation
---------- | ------- | -------
`id` | `String` | Order ID
`symbol` | `String` | Trading pairs
`side` | `String` | Trading direction（`buy`, `sell`）
`type` | `String` | Order type（`limit`，`market`）
`price` | `String` | Order Price
`amount` | `String` | Order quantity
`state` | `String` | State of order
`executed_value` | `String` | Traded
`filled_amount` | `String` | Trading volume
`fill_fees` | `String` | service fee
`created_at` | `Long` | Creation time
`source` | `String` | Source

Description of state of order：

Property  | Description
----------- | -------
`submitted` | Submission completed
`partial_filled` | Partially traded
`partial_canceled` | Partial_filled canceled
`filled` | wholly traded
`canceled` | cancellation completed
`pending_cancel` | the canceling is submitted

## Create a new order 

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

> The response results are as follows：

```json
{
  "status": 0,
  "data": "9d17a03b852e48c0b3920c7412867623"
}
```

This API is used to create new orders。

### HTTP Request

`POST https://api.fcoin.com/v2/orders`

### Request parameters

Parameter | Default | Description
--------- | ------- | -----------
symbol | Null  | trading pairs
side | Null | trading direction
type | Null  | order type
price | Null | price
amount | Null | order volume
exchange | Null | exchange
account_type | Null | Order type（margin:margin）






## Query order list

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

> The response results are as follows：

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

This API is used to query the order list.

### HTTP Request

`GET https://api.fcoin.com/v2/orders`

### Query parameters

Parameter | Default | Description
--------- | ------- | -----------
symbol |  | Trading pairs
states |  | State of Orders
before |  | query orders before a specified page
after |  | query orders after a specified page
limit |  | the number of orders in each page, the default is 20
account_type | Null | Order type（margin:margin）





## Get the specified order

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

> The response results are as follows：

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

This API is used to return the specified order details。

### HTTP Request

`GET https://api.fcoin.com/v2/orders/{order_id}`

### URL Parameters

Parameter | Description
--------- | -----------
order_id | order ID






## Application for cancellation of order

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

> The response results are as follows：

```json
{
  "status": 0,
  "msg": "string",
  "data": true
}
```

This API is used to cancel the specified order. The order canceling process is asynchronous. That is, the successful call of this API represents that the order has entered the revocation request. Further processing of matching is required before the confirmation of order canceling.

### HTTP Request

`POST https://api.fcoin.com/v2/orders/{order_id}/submit-cancel`

### URL Parameters

Parameters | Description
--------- | -----------
order_id | order ID






## Query trading records of a specified order

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

> The response results are as follows：

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

This API is used to get the trading record of the specified order

### HTTP Request

`GET https://api.fcoin.com/v2/orders/{order_id}/match-results`

### URL Parameters

Parameters | Description
--------- | -----------
order_id | order ID
