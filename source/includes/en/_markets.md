# Market

## Market overview

The market is a fully public API, which currently provides both HTTP and WebSocket APIs.
In order to ensure more timely access to the market, it is recommended to use WebSocket to access. 
For the real-time performance of the market as much as possible, the current public part can only obtain the most recent market information.  If there is a need to obtain the full amount or historical market information, please consult `support@fcoin.com`

The URL base for all HTTP requests is: `https://api.fcoin.com/v2/market`

The requested URL for all WebSocket is: `wss://api.fcoin.com/v2/ws`

Terminology will be consistent below:

- `topic` indicates the topic that has been subscribed.
- `symbol` indicates the corresponding trading currency.
- `ticker` market  and tick information include the latest transaction price, the latest  transaction volume, buy 1 sell 1, and the transaction 
- `depth` indicates the depth of market, order, handicap.
- `level` indicates the type of market depth. Such as `L20`, `L150`.
- `trade` indicates the latest transactions and latest deals.
- `candle` represents candlestick chart, candlestick, K-line.
- `resolution` indicates the type of candlestick.. Such as `M1`, `M15`.
- `base volume` indicates the base currency volume, such as the amount of btc in btcusdt.
- `quote volume` represents the volume of the quote currency, such as the volume of the usdt in btcusdt.
- `ts` indicates the time to push the server. It Is numeric field, and unix epoch in millisecond.

## WebSocket’s first connection is established.

The server sends a welcome message

> Server return

```json
{
  "type":"hello",
  "ts":1523693784042
}
```

- `ts`: push the current time of the server.

## WebSocket connection retention - heartbeat

After the WebSocket client establishes a connection with the WebSocket server, it is recommended that the WebSocket Client initiate a ping request to the server every 30 seconds (this frequency may change). If the server does not receive the client's ping request for a long time, it will actively disconnect (300s). 

### WebSocket requests

```python
import time
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
now_ms = int(time.time())
api.market.ping(now_ms)
```


> Server return

```json
{
  "type":"ping",
  "ts":1523693784042,
  "gap":112
}
```

- `gap`: the gap between the time that the push server processes this statement and the client's transmission.
- `ts`: push the current time of the server.

## Get push server time

The gap between the push server time and data transfer time can be obtained by the ts and gap values returned by the server at the time of the ping request

- gap: the gap between the time that the push server processes this statement and the client's transmission.
- ts: push the current time of the server.


## Get ticker data

To make the ticker Information Group small and fast enough, we enforced the list format.

> The description of the corresponding field of the ticker list:

```json
[
  "Latest transaction price",
  "the most recent trading volume",
  "The highest buy 1 price",
  "The biggest buy 1 volume",
  "The lowest sell 1 price",
  "The smallest sell 1 volume",
  "The transaction price before 24 hours",
  "The highest price in 24 hours",
  "The lowest price in 24 hours",
  "The base currency volume within 24 hours, such as the amount of btc in btcusdt",
  "Priced currency volume within 24 hours, such as the amount of usdt in btcusdt"
]
```


### HTTP requests

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

### WebSocket subscription

topic: `ticker.$symbol`

```python
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["ticker.ethbtc", "ticker.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> The results of a successful response to the subscription are as follows：

```json
{
  "type": "topics",
  "topics": ["ticker.ethbtc", "ticker.btcusdt"]
}
```

> Regular push results:

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



## Get the latest trading details

### HTTP Request

`GET https://api.fcoin.com/v2/market/depth/$level/$symbol`

types of `$level` included:

Type | Description
-------- | --------
`L20` | 20 -level market depth.
`L150` | 150 -level market depth.

The `L20` push time will be slightly earlier than `L100`, and the push frequency will be slightly more than `L100`, depending on the specified pressure and situation.

### WebSocket subscription

subscription topic: `depth.$level.$symbol`

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


> The results of a successful response to the subscription are as follows：

```json
{
  "type": "topics",
  "topics": ["depth.L20.ethbtc", "depth.L100.btcusdt"]
}
```

> Regular push results

The array corresponding to bids and asks must be an even number, buy (sell) 1 price, buy (sell) 1 volume, and arrange them one after the other.

```json
{
  "type": "depth.L20.ethbtc",
  "ts": 1523619211000,
  "seq": 120,
  "bids": [0.000100000, 1.000000000, 0.000010000, 1.000000000],
  "asks": [1.000000000, 1.000000000]
}
```

## Get the latest trading details

By comparing the size of the transaction id to determine whether it is an updated transaction. {trade id} Note that the transaction id of the public market does not actually correspond to the transaction id in the clearing system due to the existence of the trade to transaction process.
Even if the transaction is a record, there is no guarantee that the id will always be consistent when the latest transaction is re-acquired

PS: in the historical market, the trading id remains constant. {transaction id} is only used as a market update notification and should not be used depending on the archive.


### HTTP Request

`GET https://api.fcoin.com/v2/market/trades/$symbol`

### Query parameters(HTTP Query)

Parameter | Default | Description
--------- | ------- | -----------
before |  | Query a trade before a specified id
limit |  | the default is 20

### WebSocket gets the most recent deal

topic: `trade.$symbol`
limit: the number of most recent transactions
args: [topic, limit]

```python
import fcoin

fcoin_ws = fcoin.init_ws()
topic = "trade.ethbtc"
limit = 3
args = [topic, limit]
fcoin_ws.req(args, rep_handler)
```

> The result of a successful response to the request is as follows：

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

### WebSocket subscription

```python
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["trade.ethbtc", "trade.btcusdt"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> The results of a successful response to the subscription are as follows：

```json
{
  "type": "topics",
  "topics": ["trade.ethbtc"]
}
```


> Regular push results

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

## Get Candle information

### HTTP Request

`GET https://api.fcoin.com/v2/market/candles/$resolution/$symbol`

### Query parameters(HTTP Query)

Parameter | default | description
--------- | ------- | -----------
before |  | Query a candle before a specified id
limit |  | the default is 20

Types contained in $resolution

Type     | Description
-------- | --------
 `M1`    | 1 minute
 `M3`    | 3 minutes
 `M5`    | 5 minutes
 `M15`   | 15 minutes
 `M30`   | 30 minutes
 `H1`    | 1 hour
 `H4`    | 4 hours
 `H6`    | 6 hours
 `D1`    | 1 day
 `W1`    | 1 Week
 `MN`    | 1 month

### Candle data subscribed in Weboskcet

topic: `candle.$resolution.$symbol`

```python
import fcoin

fcoin_ws = fcoin.init_ws()
topics = ["candle.M1.ethbtc", "depth.L20.ethbtc", "trade.ethbtc"]
fcoin_ws.handle(print)
fcoin_ws.sub(topics)
```

> The results of a successful response to the subscription are as follows：


```json
{
  "type": "topics",
  "topics": ["candle.M1.ethbtc"]
}
```

> The notification message format for a regular subscription is as follows:

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
