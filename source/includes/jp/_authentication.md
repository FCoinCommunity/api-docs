# 認証

> 下記コードを使用して、ユーザー認証を行います：

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
```

FCoinはAPI キーとAPIシークレットを使用して、認証を行います。 設定センターにて，開発者にご登録してください。ご登録いただいた後、API キーとAPI シークレット情報をご利用いただけます。

FCoinのAPI利用について，公開API以外に、API キー及び署名が必要となります。




## アクセス制限

現在、アクセスの頻度はユーザーごとに100回 / 10秒，将来はサービスごと、アクセスの頻度を制限する予定です。




## API 署名

署名を行うため、あらかじめ下記データをご用意してください：

`HTTP_METHOD` + `HTTP_REQUEST_URI` + `TIMESTAMP` + `POST_BODY`

接続が確立後， Base64エンコーディングが必要です。エンコーディング後のデータに対して、 HMAC-SHA1 署名を行います。また、デジタル署名に対して、Base64エンコーディングを2回実施します。各ステップの詳細内容について、ご説明します：

<aside class="warning">
注意事項： `Base64`エンコーディングは2回実施する必要があります！
</aside>

### HTTP_METHOD

`GET`, `POST`, `DELETE`, `PUT` は必ず大文字にしてください

### HTTP_REQUEST_URI

`https://api.fcoin.com/v2/` v2 APIのリクエストの接頭辞

后面再加上真正要访问的资源路径，如 `orders?param1=value1`，最终即 `https://api.fcoin.com/v2/orders?param1=value1`

对于请求的 URI 中的参数，需要按照按照字母表排序！

即如果请求的 URI 为 `https://api.fcoin.com/v2/orders?c=value1&b=value2&a=value3`，则进行签名时，应先将请求参数按照字母表排序，最终进行签名的 URI 为 `https://api.fcoin.com/v2/orders?a=value3&b=value2&c=value1`，
请注意，原请求 URI 中的三个参数顺序为 `c, b, a`，排序后为 `a, b, c`。

### TIMESTAMP

访问 API 时的 UNIX EPOCH 时间戳，需要和服务器之间的时间差少于 30 秒

### POST_BODY

如果是 `POST` 请求，`POST` 请求数据也需要被签名，签名规则如下：

所有请求的 key 按照字母顺序排序，然后进行 url 参数化，并使用 `&` 连接。

<aside class="warning">
请注意 POST_BODY 的键值需要按照字母表排序！
</aside>

> 如果请求数据为：

```json
{
  "username": "username",
  "password": "passowrd"
}
```

> 则先将 key 按照字母排序，然后进行 url 参数化，即：

```
password=password
username=username
```

> 因为 `p` 在字母表中的排序在 `u` 之前，所以 `password` 要放在 `username` 之前，然后使用 `&` 进行连接，即：

```
password=password&username=username
```

## 完整示例

> 对于如下的请求：

```
POST https://api.fcoin.com/v2/orders

{
  "type": "limit",
  "side": "buy",
  "amount": "100.0",
  "price": "100.0",
  "symbol": "btcusdt"
}

timestamp: 1523069544359
```

> 签名前的准备数据如下：

```
POSThttps://api.fcoin.com/v2/orders1523069544359amount=100.0&price=100.0&side=buy&symbol=btcusdt&type=limit
```

> 进行 Base64 编码，得到：

```
UE9TVGh0dHBzOi8vYXBpLmZjb2luLmNvbS92Mi9vcmRlcnMxNTIzMDY5NTQ0MzU5YW1vdW50PTEwMC4wJnByaWNlPTEwMC4wJnNpZGU9YnV5JnN5bWJvbD1idGN1c2R0JnR5cGU9bGltaXQ=
```

> 拷贝在申请 API Key 时获得的秘钥（API SECRET），下面的签名结果采用 `3600d0a74aa3410fb3b1996cca2419c8` 作为示例，

> 对得到的结果使用秘钥进行 `HMAC-SHA1` 签名，并对二进制结果进行 `Base64` 编码，得到：

```
DeP6oftldIrys06uq3B7Lkh3a0U=
```

> 即生成了用于向 API 服务器进行验证的最终签名

## 参数名称

* `FC-ACCESS-KEY`
* `FC-ACCESS-SIGNATURE`
* `FC-ACCESS-TIMESTAMP`

## 说明

可以使用[开发者工具]()（暂未开放）进行在线联调测试。
