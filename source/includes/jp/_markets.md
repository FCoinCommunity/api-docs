# マーケット情報

## マーケット情報概述

マーケット情報は公開APIを通じて取得できます。現在、HTTPとWebSocketの2つのAPIを提供しています。タイムリーにマーケット情報を取得するため、WebSocketを使用してアクセスすることをお勧めします。マーケット情報のリアルタイムパフォーマンスを可能な限り高めるために、現在の公開している部分は最新のマーケットデータのみを取得することができます。もし全部の情報、または履歴情報を取得する必要がある場合は、support@fcoin.comまでご連絡ください。

すべての HTTP リクエストのURLベースは：https://api.fcoin.com/v2/market です

すべての WebSocket リクエストのURLは: wss://api.fcoin.com/v2/ws です

用語は以下のように統一されます:

- `topic` サブスクリプションのテーマを示します。
- `symbol` 対応する取引通貨を示します。すべての通貨区分のトピックはトピックの最後に表示されます。
- `ticker` マーケット情報のtickデータ、最新の買い売りの約定価格と出来高、買い注文の最高値と売り注文の最安値、24時間の出来高が含まれます。
- `depth` 板の厚さ、相場を示します。
- `level` 板の厚さのタイプを示します。例えば、`L20`, `L100`。
- `trade` 最新の成約と取引を示します。
- `candle` チャート、ローソク足、平均足を示します。
- `resolution` チャートの種類を示します。例えば、`M1`, `M15`。
- `base volume` 基軸通貨の出来高を示します。例えば、 btcusdt 中 btc のボリューム。
- `quote volume` 評価通貨の出来高を示します。例えば、 btcusdt 中 usdt のボリューム。
- `ts` プッシュサーバーの時刻を示します。ミリ秒単位の数値フィールドです。 unix epoch in millisecond.

## WebSocket 初回接続の確立

接続ができたら、サーバーからウェルカムメッセージを送信します。

> 接続成功後、サーバーからメッセージを返します:

```json
{
  "type":"hello",
  "ts":1523693784042
}
```

> * `ts`: プッシュサーバーの現在の時点。

## WebSocket 接続の維持 - heartbeat

```python
# WebSocket サーバーに ping を送信し、ハートビートを維持します。
import time
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
now_ms = int(time.time())
api.market.ping(now_ms)
```


WebSocket クライアント側は WebSocket サーバーとの接続を確立後、お勧めとして、WebSocket Client が30sごとに（この頻度は変化する可能性があります）、サーバーへ ping リクエストを送信します。サーバーがクライアントのping要求を長時間受信しなかった場合、サーバーはアクティブに接続を切断します（300s）。

### WebSocket リクエスト

**ping リクエスト**の送信: `{"cmd":"ping","args":[$client_ts],"id":"$client_id"}`

* `client_id`: クライアントが現在のリクエストで指定されたカスタムidであり、サーバーはそのままで返します。
* `client_ts`: クライアントの現在の時点

> ping リクエストの例：

```json
{"cmd":"ping","args":[1540557696867],"id":"sample.client.id"}
```

> ping リクエストが成功になり、サーバーから以下の情報を返しました：

```json
{
  "id":"sample.client.id",
  "type":"ping",
  "ts":1523693784042,
  "gap":112
}
```

> * `gap`: プッシュサーバーがこのリクエストを処理する時間と、ユーザーまで送信する時差。
> * `ts`:  プッシュサーバーの現在の時点。

<aside class="notice">
tip: ping リクエストでサーバーから返した ts 和 gap を通して、プッシュサーバーの時間とユーザーまで送信する時差を取得することができます。
</aside>

## WebSocket サブスクリプション

 **sub リクエスト**の送信: `{"cmd":"sub","args":["$topic", ...],"id":"$client_id"}`

* `client_id`: クライアントが現在のリクエストで指定されたカスタムidであり、サーバーはそのままで返します。
* `topic`: サブスクリプション待ちの topic が複数である場合は、特殊記号`,`で区切ってください。

> sub リクエストの例（単一 topic）：

```json
{"cmd":"sub","args":["ticker.ethbtc"]}
```

> sub リクエストの例（複数 topic）：

```json
{"cmd":"sub","args":["ticker.ethbtc", "ticker.btcusdt"]}
```

> サブスクリプション成功した場合の応答結果は下記のとおりです：

```json
{
  "type": "topics",
  "topics": ["ticker.ethbtc", "ticker.btcusdt"]
}
```

> サブスクリプション失敗した場合の応答結果は下記のとおりです：

```json
{
  "id":"invalid_topics_sample",
  "status":41002,
  "msg":"invalid sub topic, xxx.M1.xxx"
}
```

## ticker データの取得

ticker 情報ブロックを十分に小さく、迅速に取得するために、強制的にリスト形式を使用しています。

> ticker リストの対応するフィールドの意味説明：

```json
[
  "最新取引価格",
  "最新の成約高",
  "買い注文価格の最高値",
  "買い注文取引量の最高値",
  "売り注文価格の最安値",
  "売り注文取引量の最安値",
  "24時間前の成約価格",
  "24時間価格の最高値",
  "24時間価格の最安値",
  "24時間基軸通貨の取引量。例えば、btcusdt の btc のボリューム",
  "24時間評価通貨の取引量。例えば、btcusdt の usdt のボリューム"
]
```


### HTTP リクエスト

`GET https://api.fcoin.com/v2/market/ticker/$symbol`

```python
#  ticker データの取得
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
api.market.get_ticker("ethbtc")
```

> HTTP リクエストの応答結果は下記の通りです：

```json
{
  "status": 0,
  "data": {
    "type": "ticker.btcusdt",
    "seq": 680035,
    "ticker": [
      7140.890000000000000000,
      1.000000000000000000,
      7131.330000000,
      233.524600000,
      7140.890000000,
      225.495049866,
      7140.890000000,
      7140.890000000,
      7140.890000000,
      1.000000000,
      7140.890000000000000000
    ]
  }
}
```

### WebSocket サブスクリプション

 **sub リクエスト**の送信、topic: `ticker.$symbol`  ( `WebSocket サブスクリプション`をご参照ください)

```python
# ticker データのサブスクリプション
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["ticker.ethbtc", "ticker.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> WebSocket サブスクリプションの通知結果は下記の通りです：

```json
{
  "type": "ticker.btcusdt",
  "seq": 680035,
  "ticker": [
    7140.890000000000000000,
    1.000000000000000000,
    7131.330000000,
    233.524600000,
    7140.890000000,
    225.495049866,
    7140.890000000,
    7140.890000000,
    7140.890000000,
    1.000000000,
    7140.890000000000000000
  ]
}
```



## 最新の厚み詳細の取得

### HTTP リクエスト

`GET https://api.fcoin.com/v2/market/depth/$level/$symbol`

`$level` 含まれる種類(大文字と小文字にご注意ください)：

種類 | 説明
-------- | --------
`L20` | 20 番目の注文までの明細
`L150` | 150 番目の注文までの明細

その中で、 `L20` のプッシュ時間が `L150`より少し早くなり、プッシュの頻度が `L150`より少し多くなります。具体的なストレスや状況により決められます。自身のニーズによって、ご利用ください。

> HTTP リクエストの応答結果は下記の通りです：

```json
{
  "status":0,
  "data":{
    "type": "depth.L20.ethbtc",
    "ts": 1523619211000,
    "seq": 120,
    "bids": [0.000100000, 1.000000000, 0.000010000, 1.000000000],
    "asks": [1.000000000, 1.000000000]
  }
}
```

### WebSocket サブスクリプション

```python
# WebSocket サブスクリプションのデプス詳細
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["depth.L20.ethbtc", "depth.L150.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

```javascript
// WebSocket サブスクリプションのデプス詳細
const fcoin = require('fcoin');

let fcoin_ws = fcoin.init_ws()
topics = ["depth.L20.ethbtc", "depth.L150.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

 **sub リクエスト**の送信，topic: `depth.$level.$symbol`   ( `WebSocket サブスクリプション`をご参照ください)

> WebSocket サブスクリプションの通知結果が下記の通りです：

```json
{
  "type": "depth.L20.ethbtc",
  "ts": 1523619211000,
  "seq": 120,
  "bids": [0.000100000, 1.000000000, 0.000010000, 1.000000000],
  "asks": [1.000000000, 1.000000000]
}
```

> bids 和 asks に対応する配列は偶数の項目であります。買い（売り）注文１の価格、買い（売り）注文１の数量のように順序に並びます。


## 最新の取引明細の取得

取引 id のサイズを比較することによって、最新の取引かどうかを判断できます。{trade id}
通常、 trade から transaction までのプロセスがあるため、公開マーケットの取引 id は、決済システム中の取引 id と実際には一致しないことにご注意ください。
取引記録が１つであっても、最新の取引が再取得されたときに、 id が常に一致しているという保証はできません。

PS:過去のマーケット情報では、取引 id が１つであることが保証できます。{transaction id} ここはマーケットの更新通知としてのみ使用され、アーカイブに使用しないでください。


```python
# WebSocket 最新の取引明細を照会するリクエスト
import fcoin

fcoin_ws = fcoin.init_ws()
topic = "trade.ethbtc"
limit = 3
args = [topic, limit]
fcoin_ws.req(args, rep_handler)
```

```python
# WebSocket 最新の取引明細のサブスクリプション
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["trade.ethbtc", "trade.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```


### HTTP リクエスト

`GET https://api.fcoin.com/v2/market/trades/$symbol`

#### ペラメータの紹介(HTTP リクエスト)

ペラメータ | デフォルト値 | 説明
--------- | ------- | -----------
before |  | ある id より小さい id の Candle を照会する
limit |  | デフォルトは 20 条

### WebSocket リクエスト

 **req リクエスト**の送信: `{"cmd":"req", "args":["$topic", limit],"id":"$client_id"}`

* `client_id`: クライアントが現在のリクエストで指定されたカスタムidであり、サーバーはそのままで返します。
* `topic`: `trade.$symbol`
* `limit`: 取得する必要がある最近の成約数

> WebSocket リクエストが成功した場合の応答結果が下記の通りです：

```json
{
  "id":null,
  "ts":1523693400329,
  "data":[
    {
      "amount":1.000000000,
      "ts":1523419946174,
      "id":76000,
      "side":"sell",
      "price":4.000000000
    },
    {
      "amount":1.000000000,
      "ts":1523419114272,
      "id":74000,
      "side":"sell",
      "price":4.000000000
    },
    {
      "amount":1.000000000,
      "ts":1523415182356,
      "id":71000,
      "side":"sell",
      "price":3.000000000
    }
  ]
}
```

### WebSocket サブスクリプション

 **sub リクエスト**の送信、topic: `trade.$symbol`   ( `WebSocket サブスクリプション`をご参照ください)

* `symbol`: 対応の取引ペア

> WebSocket サブスクリプションの通知結果が下記の通りです：

```json
{
  "type":"trade.ethbtc",
  "id":76000,
  "amount":1.000000000,
  "ts":1523419946174,
  "side":"sell",
  "price":4.000000000
}
```

##  Candle 情報の取得

### HTTP リクエスト

`GET https://api.fcoin.com/v2/market/candles/$resolution/$symbol`

#### パラメータの照会(HTTP リクエスト)

ペラメータ | デフォルト値 | 説明
--------- | ------- | -----------
before |  | ある id より小さい id の Candle を照会する
limit |  | デフォルトは 20 条

$resolution 含まれる種類（大文字と小文字にご注意ください）：

種類     | 説明
-------- | --------
 `M1`    | 1 分
 `M3`    | 3 分
 `M5`    | 5 分
 `M15`   | 15 分
 `M30`   | 30 分
 `H1`    | 1 時間
 `H4`    | 4 時間
 `H6`    | 6 時間
 `D1`    | 1 日
 `W1`    | 1 週
 `MN`    | 1 ヶ月

### WebSocket リクエスト

 **req リクエスト**の送信: `{"cmd":"req","args":["$topic",limit,before],"id":"$client_id"}`

- `client_id`: クライアントが現在のリクエストで指定されたカスタムidであり、サーバーはそのままで返します
- `topic`: `candle.$resolution.$symbol`
- `limit`: 取得する必要がある candle の数
- `before`: ある id より小さい id の Candle を照会する

> WebSocket リクエストが成功した場合の応答結果が下記の通りです：

```json
{
  "id":"candle.btcusdt.M1",
  "data":[
    {
      "id":1540809840,
      "seq":24793830600000,
      "high":6491.74,
      "low":6489.24,
      "open":6491.24,
      "close":6490.07,
      "count":26,
      "base_vol":8.2221,
      "quote_vol":53371.531286
    },
    {
      "id":1540809900,
      "seq":24793879800000,
      "high":6490.47,
      "low":6487.62,
      "open":6490.09,
      "close":6487.62,
      "count":23,
      "base_vol":10.8527,
      "quote_vol":70430.840624
    }
  ]
}
```

### Weboskcet サブスクリプション

 **sub リクエスト**の送信，topic: `candle.$resolution.$symbol`   ( `WebSocket サブスクリプショ`をご参照ください)

* `resolution`： HTTP からの resolution ペラメータへのリクエストに相当します

```python
# WebSocket サブスクリプション candle データ
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["candle.M1.ethbtc"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> WebSocket サブスクリプションの通知結果が下記の通りです：

```json
{
  "type":"candle.M1.ethbtc",
  "id":1523691480,
  "seq":11400000,
  "open":2.000000000,
  "close":2.000000000,
  "high":2.000000000,
  "low":2.000000000,
  "count":0,
  "base_vol":0,
  "quote_vol":0
}
```
