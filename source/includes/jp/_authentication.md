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

その後に、アクセスしたいリソースパスを記入してください。例： `orders?param1=value1`の場合は，最終形は `https://api.fcoin.com/v2/orders?param1=value1`となります

リクエストされたURIのパラメータについては、アルファベット順に並べ替える必要があります！

すなわち、リクエストされたURIは`https://api.fcoin.com/v2/orders?c=value1&b=value2&a=value3`の場合，デジタル署名する際に、リクエスト・パラメータをアルファベット順にソートさせ、デジタル署名の最終URIは `https://api.fcoin.com/v2/orders?a=value3&b=value2&c=value1`となります。 元のリクエスト中には、URI中の三つのパラメータの順番は `c`, `b`, `a`だが，ソート後、順番は `a`, `b`, `c`になっております。ご注意ください。

### TIMESTAMP

APIを利用してアクセスする時のUNIX EPOCHタイムスタンプは、サーバーとの時間差は30秒以内でなければなりません。

### POST_BODY

POST リクエストの場合は，POST リクエストデータへのデジタル署名も必要です。デジタル署名のルールはご説明します：

リクエストされたすべてのキーはアルファベット順に並べ替えられた後、urlパラメータ化されます。 &を使用して、繋ぎます。

<aside class="warning">
POST_BODYのキーの値は、アルファベット順にソートする必要があります。ご注意ください！
</aside>

> 下記リクエストデータの場合は：

```json
{
  "username": "username",
  "password": "passowrd"
}
```

> 先に、キーをアルファベット順にソートし、urlパラメータ化します。すなわち：

```
password=password
username=username
```

> アルファベットの順番には、`p`はuの前にあるため、`password`は`username`の前に置かれるべき、その次、＆を使用して接続します。すなわち：

```
password=password&username=username
```

## デモンストレーション

> 下記のリクエストの場合は：

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

> デジタル署名を行うため、あらかじめ下記データをご用意してください：

```
POSThttps://api.fcoin.com/v2/orders1523069544359amount=100.0&price=100.0&side=buy&symbol=btcusdt&type=limit
```

> Base64エンコーディングし、下記データになります：

```
UE9TVGh0dHBzOi8vYXBpLmZjb2luLmNvbS92Mi9vcmRlcnMxNTIzMDY5NTQ0MzU5YW1vdW50PTEwMC4wJnByaWNlPTEwMC4wJnNpZGU9YnV5JnN5bWJvbD1idGN1c2R0JnR5cGU9bGltaXQ=
```

> API キーを申請した際に取得したキー（API シークレット）をコピーし，以下のデジタル署名結果は、例として `3600d0a74aa3410fb3b1996cca2419c8` を使用しています，

> 結果に対して、秘密鍵を使用して、 `HMAC-SHA1` デジタル署名し、バイナリ結果に対して`Base64` エンコーディング後、下記になります：

```
DeP6oftldIrys06uq3B7Lkh3a0U=
```

> すなわち、APIサーバーへ検証するための最終的なデジタル署名が生成されます

## パラメータ名

* `FC-ACCESS-KEY`
* `FC-ACCESS-SIGNATURE`
* `FC-ACCESS-TIMESTAMP`

## 説明

デベロッパーツール（準備中）を使用して、オンライン検証を行えます。
