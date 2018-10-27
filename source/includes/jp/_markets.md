# マーケット情報

## マーケット情報概述

マーケット情報は公開APIを通じて取得できます。現在、HTTPとWebSocketの2つのAPIを提供しています。 タイムリーにマーケット情報を取得するため、WebSocketを使用してアクセスすることをお勧めします。 マーケット情報のリアルタイムパフォーマンスを可能な限り高めるために、現在の公開している部分は最新のマーケットデータのみを取得することができます。もし全部の情報、または履歴情報を取得する必要がある場合は、`support@fcoin.com`までご連絡ください

すべてのHTTP リクエストのURLベースは：`https://api.fcoin.com/v2/market`です

すべてのWebSocket リクエストのURLは: `wss://api.fcoin.com/v2/ws`です

用語は以下のように統一されます:

- `topic` サブスクリプションのテーマを示します
- `symbol` 対応する取引通貨を示します。すべての通貨区分のトピックはトピックの最後に表示されます.
- `ticker` マーケット情報のtickデータ、最新の買い売りの約定価格と出来高、24時間の出来高が含まれます.
- `depth` デプス情報、買い注文と売り注文の数量、ハンディキャップを示します.
- `level` デプス情報のタイプを示します。例えば、 `L20`, `L150`.
- `trade` 最新の約定と取引を示します.
- `candle` ローソクチャート、ローソク足、平均足を示します.
- `resolution` ローソクチャートの種類を示します。 例えば、 `M1`, `M15`.
- `base volume` 基準通貨の出来高を示します。例えば、btcusdtのbtcのボリューム.
- `quote volume` 評価通貨の出来高を示します。例えば、btcusdtのusdtのボリューム
- `ts` プッシュサーバーの時刻を示します。ミリ秒単位の数値フィールドです, unix epoch in millisecond.

## WebSocket 初回接続の確立

サーバーからウェルカムメッセージを送信します

> サーバーからのレスポンス

```json
{
  "type":"hello",
  "ts":1523693784042
}
```

- `ts`: サーバーの現在時間をプッシュします.

## WebSocket 接続の維持 - heartbeat

WebSocket クライアント側はWebSocket サーバーとの接続を確立後、お勧めとして、WebSocket Client 30sごとに（この頻度は変化する可能性があります）、サーバーへping リクエストを送ります。サーバーがクライアントのping要求を長時間受信しなかった場合、サーバーはアクティブに切断します（300s）。

### WebSocket 请求

```python
import time
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
now_ms = int(time.time())
api.market.ping(now_ms)
```


> サーバーからのレスポンス

```json
{
  "type":"ping",
  "ts":1523693784042,
  "gap":112
}
```

- `gap`: プッシュサーバーがこのステートメントを処理する時間とクライアント側伝送との時間差.
- `ts`: サーバーの現在時間をプッシュします.

## プッシュサーバー時間の取得

プッシュサーバーの時刻とデータ転送の時間差は、pingリクエスト時のサーバーから返されたtsとgapの値によって取得できます

- gap: プッシュサーバーがこのステートメントを処理する時間とクライアント側伝送との時間差.
- ts: サーバーの現在時間をプッシュします.


## tickerデータの取得

ticker情報ブロックを十分に小さく、迅速に取得するために、強制的にリスト表式を使用しています.

> ticker リスト表の該当フィールドの意味説明を示します:

```json
[
  "最新取引価格",
  "最新約定の出来高",
  "最高取引価格（買）",
  "最大出来高（買）",
  "最安取引価格（売）",
  "最小出来高（売）",
  "24時間前取引価格",
  "24時間内最高価格",
  "24時間内最安価格",
  "24時間内基準通貨出来高。例えば、btcusdtのbtcのボリューム",
  "24時間内評価通貨出来高。例えば、btcusdtのusdtのボリューム"
]
```


### HTTP リクエスト

`GET https://api.fcoin.com/v2/market/ticker/$symbol`

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
api.market.get_ticker("ethbtc")

```

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

topic: `ticker.$symbol`

```python
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["ticker.ethbtc", "ticker.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> サブスクリプション成功した場合のレスポンスは次のとおりです：

```json
{
  "type": "topics",
  "topics": ["ticker.ethbtc", "ticker.btcusdt"]
}
```

> 通常のサブスクリプションの通知メッセージのフォーマットは次のとおりです:

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



## 詳細マーケット情報取得

### HTTP Request

`GET https://api.fcoin.com/v2/market/depth/$level/$symbol`

`$level` 下記種類を含みます:

タイプ | 説明
-------- | --------
`L20` | レベル20のマーケット情報.
`L150` | レベル100のマーケット情報.
`full` | 全レベルのマーケット情報、送信時間と通知保証は.

その中、 L20の通知時間は L100より若干早く、通知頻度は L100より若干多いです。実際の圧力と状況に基づきます。必要に応じて使用してください.

### WebSocket サブスクリプション

サブスクリプション topic: `depth.$level.$symbol`

```python
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["depth.L20.ethbtc", "depth.L100.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

```javascript
const fcoin = require('fcoin');

let fcoin_ws = fcoin.init_ws()
topics = ["depth.L20.ethbtc", "depth.L100.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```


> サブスクリプション成功した場合のレスポンスは次のとおりです：

```json
{
  "type": "topics",
  "topics": ["depth.L20.ethbtc", "depth.L100.btcusdt"]
}
```

> 通常の通知結果は下記のとおりです

bidsとasksに対応する配列は、偶数でなければならず、1つの価格を購入（売却）し、1つの数量を購入（売却）し、順次後ろに並べる必要があります.

```json
{
  "type": "depth.L20.ethbtc",
  "ts": 1523619211000,
  "seq": 120,
  "bids": [0.000100000, 1.000000000, 0.000010000, 1.000000000],
  "asks": [1.000000000, 1.000000000]
}
```

## 最新の出来高明細の取得

取引idのサイズを比較することによって、更新された取引かどうかを判断できます。{trade id} 通常、traceからtransactionまでのプロセスがあるため、公開”行情”の取引idは、決済システム中の取引idと実際には一致しないことにご留意いただきたい。 取引記録が１つであっても、最新の取引が再取得されたときに、IDが常に一致しているという保証はできません.

PS: 過去のマーケット情報では、取引IDが１つであることが保証できます。{transaction id} ここは”行情”の更新通知としてのみ使用され、アーカイブに使用しないでください.


### HTTP Request

`GET https://api.fcoin.com/v2/market/trades/$symbol`

### パラメータの照会(HTTP Query)

パラメータ | デフォルト値 | 説明
--------- | ------- | -----------
before |  | あるidの前のTradeの照会
limit |  | デフォルトは20条

### WebSocket 最新の取引データの照会

topic: `trade.$symbol`
limit: 最新の取引ボリューム
args: [topic, limit]

```python
import fcoin

fcoin_ws = fcoin.init_ws()
topic = "trade.ethbtc"
limit = 3
args = [topic, limit]
fcoin_ws.req(args, rep_handler)
```

> リクエスト成功した際のレスポンスは下記のとおりです：

```json
{"id":null,
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

```python
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["trade.ethbtc", "trade.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> 订阅成功的响应结果如下：

```json
{
  "type": "topics",
  "topics": ["trade.ethbtc"]
}
```


> 通常の通知結果は下記のとおりです

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

## Candle情報の取得

### HTTP Request

`GET https://api.fcoin.com/v2/market/candles/$resolution/$symbol`

### パラメータの照会(HTTP Query)

パラメータ | デフォルト値 | 説明
--------- | ------- | -----------
before |  | 查あるidより小さいidのCandle情報
limit |  | デフォルトは20条

$resolution 含める種類

タイプ     | 説明
-------- | --------
 `M1`    | 1 分間
 `M3`    | 3 分間
 `M5`    | 5 分間
 `M15`   | 15 分間
 `M30`   | 30 分間
 `H1`    | 1 時間
 `H4`    | 4 時間
 `H6`    | 6 時間
 `D1`    | 1 日間
 `W1`    | 1 週間
 `MN`    | 1 ヶ月

### Weboskcet Candle情報のサブスクリプション

topic: `candle.$resolution.$symbol`

```python
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["candle.M1.ethbtc", "depth.L20.ethbtc", "trade.ethbtc"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> サブスクリプション成功した場合のレスポンスは次のとおりです：


```json
{
  "type": "topics",
  "topics": ["candle.M1.ethbtc"]
}
```

> 通常のサブスクリプションの通知メッセージのフォーマットは次のとおりです:

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
