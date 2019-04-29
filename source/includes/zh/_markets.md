# 行情

## 行情概述

行情是一个全公开的 API, 当前同时提供了 HTTP 和 WebSocket 的 API.
为确保可以更及时的获得行情, 推荐使用 WebSocket 进行接入.
为尽可能行情的实时性能, 当前公开部分只能获取最近一段时间的行情, 如果有需要获取全量或者历史行情, 请咨询 `support@fcoin.com`

所有 HTTP 请求的 URL base 为: `https://api.fcoin.com/v2/market`

所有 WebSocket 请求的 URL 为: `wss://api.fcoin.com/v2/ws`

下文会统一术语:

- `topic` 表示订阅的主题
- `symbol` 表示对应交易币种. 所有币种区分的 topic 都在 topic 末尾.
- `ticker` 行情 tick 信息, 包含最新成交价, 最新成交量, 买一卖一, 近 24 小时成交量.
- `depth` 表示行情深度, 买卖盘, 盘口.
- `level` 表示行情深度类型. 如 `L20`, `L100`.
- `trade` 表示最新成交, 最新交易.
- `candle` 表示蜡烛图, 蜡烛棒, K 线.
- `resolution` 表示蜡烛图的种类. 如 `M1`, `M15`.
- `base volume` 表示基准货币成交量, 如 btcusdt 中 btc 的量.
- `quote volume` 表示计价货币成交量, 如 btcusdt 中 usdt 的量
- `ts` 表示推送服务器的时间. 是毫秒为单位的数字型字段, unix epoch in millisecond.

## WebSocket 首次建立连接

连接成功服务器会发送一个欢迎信息

> 连接成功后服务器返回信息:

```json
{
  "type":"hello",
  "ts":1523693784042
}
```

> * `ts`: 推送服务器当前的时间.

## WebSocket 连接保持 - heartbeat

```python
# WebSocket 向服务端发送 ping 维持心跳
import time
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
now_ms = int(time.time())
api.market.ping(now_ms)
```


WebSocket 客户端和 WebSocket 服务器建立连接之后，推荐 WebSocket Client 每隔 *30s*（这个频率可能会变化） 向服务器发起一次 ping 请求，如果服务器长时间没有接收到客户端的 ping 请求将会主动断开连接（300s）。

### WebSocket 请求

发送 **ping 指令**: `{"cmd":"ping","args":[$client_ts],"id":"$client_id"}`

* `client_id`: 客户端为当前请求指定的自定义 id，服务器端会原样返回
* `client_ts`: 客户端当前的时间

> ping 指令请求示例：

```json
{"cmd":"ping","args":[1540557696867],"id":"sample.client.id"}
```

> ping 指令成功服务器返回：

```json
{
  "id":"sample.client.id",
  "type":"ping",
  "ts":1523693784042,
  "gap":112
}
```

> * `gap`: 推送服务器处理此语句的时间和客户端传输的时间差.
> * `ts`:  推送服务器当前的时间.

<aside class="notice">
tip: 可以通过 ping 请求时服务器返回的 ts 和 gap 值获取推送服务器时间和数据传输时间差
</aside>

## WebSocket 订阅

发送 **sub 指令**: `{"cmd":"sub","args":["$topic", ...],"id":"$client_id"}`

* `client_id`: 客户端为当前请求指定的自定义 id，服务器端会原样返回
* `topic`: 待订阅的 topic，多个请用英文逗号`,`分隔，最多可以订阅20个

> sub 指令请求示例（单 topic）：

```json
{"cmd":"sub","args":["ticker.ethbtc"]}
```

> sub 指令请求示例（多 topic）：

```json
{"cmd":"sub","args":["ticker.ethbtc", "ticker.btcusdt"]}
```

> 订阅成功的响应结果如下：

```json
{
  "type": "topics",
  "topics": ["ticker.ethbtc", "ticker.btcusdt"]
}
```

> 订阅失败的响应结果如下：

```json
{
  "id":"invalid_topics_sample",
  "status":41002,
  "msg":"invalid sub topic, xxx.M1.xxx"
}
```

## 获取 ticker 数据

为了使得 ticker 信息组足够小和快, 我们强制使用了列表格式.

> ticker 列表对应字段含义说明:

```json
[
  "最新成交价",
  "最近一笔成交的成交量",
  "最大买一价",
  "最大买一量",
  "最小卖一价",
  "最小卖一量",
  "24小时前成交价",
  "24小时内最高价",
  "24小时内最低价",
  "24小时内基准货币成交量, 如 btcusdt 中 btc 的量",
  "24小时内计价货币成交量, 如 btcusdt 中 usdt 的量"
]
```


### HTTP 请求

`GET https://api.fcoin.com/v2/market/ticker/$symbol`

```python
# 获取 ticker 数据
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
api.market.get_ticker("ethbtc")
```

> HTTP 请求响应结果如下：

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

### WebSocket 订阅

发送 **sub 指令**，topic: `ticker.$symbol`  (请参考 `WebSocket 订阅`)

```python
# 订阅 ticker 数据
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["ticker.ethbtc", "ticker.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> WebSocket 订阅的通知消息结果如下:

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



## 获取最新的深度明细

### HTTP 请求

`GET https://api.fcoin.com/v2/market/depth/$level/$symbol`

`$level` 包含的种类(大小写敏感)：

类型 | 说明
-------- | --------
`L20` | 20 档行情深度.
`L150` | 150 档行情深度. 

其中 `L20` 的推送时间会略早于 `L150`, 推送频次会略多于 `L150`, 看具体的压力和情况. 此处请按需使用.

> HTTP 请求响应结果如下：

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

### WebSocket 订阅

```python
# WebSocket 订阅深度明细
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["depth.L20.ethbtc", "depth.L150.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

```javascript
// WebSocket 订阅深度明细
const fcoin = require('fcoin');

let fcoin_ws = fcoin.init_ws()
topics = ["depth.L20.ethbtc", "depth.L150.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

发送 **sub 指令**，topic: `depth.$level.$symbol`   (请参考 `WebSocket 订阅`)

> WebSocket 订阅的通知消息结果如下:

```json
{
  "type": "depth.L20.ethbtc",
  "ts": 1523619211000,
  "seq": 120,
  "bids": [0.000100000, 1.000000000, 0.000010000, 1.000000000],
  "asks": [1.000000000, 1.000000000]
}
```

> bids 和 asks 对应的数组一定是偶数条目, 买(卖)1价, 买(卖)1量, 依次往后排列.



## 获取最新的成交明细

通过对比其中的成交 id 大小才能决定是否是更新的成交.{trade id}
需要注意, 常规由于 trade 到 transaction 过程的存在, 公开行情的成交 id 并不实际对应清算系统中的成交 id.
即使成交是一条记录, 也无法保证最新成交在重新获取时候 id 永远保持一致.

PS: 历史行情中, 是可以保证成交 id 保持恒定. {transaction id} 此处只作为行情更新通知, 不应依赖归档使用.


```python
# WebSocket 请求获取最近的成交明细
import fcoin

fcoin_ws = fcoin.init_ws()
topic = "trade.ethbtc"
limit = 3
args = [topic, limit]
fcoin_ws.req(args, rep_handler)
```

```python
# WebSocket 订阅最近的成交明细
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["trade.ethbtc", "trade.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```


### HTTP 请求

`GET https://api.fcoin.com/v2/market/trades/$symbol`

#### 查询参数(HTTP 请求)

参数 | 默认值 | 描述
--------- | ------- | -----------
before |  | 查询某个 id 之前的 Trade
limit |  | 默认为 20 条

### WebSocket 请求

发送 **req 指令**: `{"cmd":"req", "args":["$topic", limit],"id":"$client_id"}`

* `client_id`: 客户端为当前请求指定的自定义 id，服务器端会原样返回
* `topic`: `trade.$symbol`
* `limit`: 需要获取的最近的成交条数

> WebSocket 请求成功的响应结果如下：

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

### WebSocket 订阅

发送 **sub 指令**，topic: `trade.$symbol`   (请参考 `WebSocket 订阅`)

* `symbol`: 对应的交易对

> WebSocket 订阅的通知消息结果如下:

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

## 获取 Candle 信息

### HTTP 请求

`GET https://api.fcoin.com/v2/market/candles/$resolution/$symbol`

#### 查询参数(HTTP 请求)

参数 | 默认值 | 描述
--------- | ------- | -----------
before |  | 查询某个 id 之前的 Candle
limit |  | 默认为 20 条

$resolution 包含的种类(大小写敏感)：

类型     | 说明
-------- | --------
 `M1`    | 1 分钟
 `M3`    | 3 分钟
 `M5`    | 5 分钟
 `M15`   | 15 分钟
 `M30`   | 30 分钟
 `H1`    | 1 小时
 `H4`    | 4 小时
 `H6`    | 6 小时
 `D1`    | 1 日
 `W1`    | 1 周
 `MN`    | 1 月

### WebSocket 请求

发送 **req 指令**: `{"cmd":"req","args":["$topic",limit,before],"id":"$client_id"}`

- `client_id`: 客户端为当前请求指定的自定义 id，服务器端会原样返回
- `topic`: `candle.$resolution.$symbol`
- `limit`: 需要获取的 candle 条数
- `before`: 查询某个 id 之前的 Candle

> WebSocket 请求成功的响应结果如下：

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

### Weboskcet 订阅

发送 **sub 指令**，topic: `candle.$resolution.$symbol`   (请参考 `WebSocket 订阅`)

* `resolution`： 同 HTTP 请求 resolution 参数

```python
# WebSocket 订阅 candle 数据
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["candle.M1.ethbtc"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> WebSocket 订阅的通知消息结果如下:

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



## 获取所有的ticker数据

### HTTP 请求

`GET https://api.fcoin.com/v2/market/all-tickers`

#### 查询参数(HTTP 请求)

无