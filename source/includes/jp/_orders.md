# 注文

## 注文モデル説明

注文モデルは、以下の属性によって構成されます：

属性 | タイプ	 | 内容説明
---------- | ------- | -------
`id` | `String` | 注文 ID
`symbol` | `String` | 通貨ペア
`side` | `String` | 取引方向（`buy`, `sell`）
`type` | `String` | 注文タイプ（`limit`，`market`）
`price` | `String` | 注文価格
`amount` | `String` | 注文ボリューム
`state` | `String` | 注文ステータス
`executed_value` | `String` | 約定
`filled_amount` | `String` | 出来高
`fill_fees` | `String` | 手数料
`created_at` | `Long` | 注文日時
`source` | `String` | ソース

注文ステータス説明：

属性  | 内容説明
----------- | -------
`submitted` | 注文中
`partial_filled` | 一部約定
`partial_canceled` | 一部キャンセル
`filled` | 約定
`canceled` | キャンセル
`pending_cancel` | キャンセルリクエスト中

## 新規注文

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

> レスポンス結果は下記のとおりです：

```json
{
  "status": 0,
  "data": "9d17a03b852e48c0b3920c7412867623"
}
```

このAPIは新規注文を発注します。

### HTTP Request

`POST https://api.fcoin.com/v2/orders`

### リクエスト・パラメータ

パラメータ | デフォルト値 | 説明
--------- | ------- | -----------
symbol | 無  | 通貨ペア
side | 無 | 取引方向
type | 無  | 注文タイプ
price | 無 | 価格
amount | 無 | 発注量
exchage | 無 | 取引ゾーン
account_type | 無 | 注文タイプ（レバレッジ：margin）






## 注文リストの照会

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

> レスポンス結果は下記のとおりです：

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

このAPIは注文リストの照会に使用されます。

### HTTP Request

`GET https://api.fcoin.com/v2/orders`

### 照会パラメータ

パラメータ | デフォルト値 | 説明
--------- | ------- | -----------
symbol |  | 通貨ペア
states |  | 注文ステータス
before |  | あるページの前の
after |  | あるページの後の注文の照会
limit |  | ページごとの注文量、デフォルトは20条





## 特定注文の取得

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

> レスポンス結果は下記のとおりです：

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

このAPIは指定された注文の詳細を取得します。

### HTTP Request

`GET https://api.fcoin.com/v2/orders/{order_id}`

### URL パラメータ

パラメータ | 説明
--------- | -----------
order_id | 注文 ID






## 注文キャンセルのリクエスト

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

> レスポンス結果は下記のとおりです：

```json
{
  "status": 0,
  "msg": "string",
  "data": true
}
```

このAPIは指定された注文をキャンセルするために、使用されます。注文のキャンセル・プロセスは非同期です。つまり、このAPIの呼び出しが成功した場合は、注文はキャンセルリクエストのプロセスに入っていますが、注文の取り消しを確認するためにマッチング処理を待つ必要があります。

### HTTP Request

`POST https://api.fcoin.com/v2/orders/{order_id}/submit-cancel`

### URL パラメータ

パラメータ | 解釈
--------- | -----------
order_id | 注文 ID






## 指定されている注文の取引記録の照会

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

> レスポンス結果は下記のとおりです：

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

このAPIは指定されている注文の取引記録の取得に使用されます

### HTTP Request

`GET https://api.fcoin.com/v2/orders/{order_id}/match-results`

### URL パラメータ

パラメータ | 解釈
--------- | -----------
order_id | 注文 ID
