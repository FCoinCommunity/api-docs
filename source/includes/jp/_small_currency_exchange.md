# 小額通貨の両替

- 両替履歴の状態に関する説明
 
 
| 属性  |  説明  |
|:------|:------:|
|WAITING_TRANSFER_TO_SYSTEM|システムアカウントへ振り替え待ち
|WAITING_REFUND|ユーザーに返却待ち
|FAIL|両替失敗
|WAITING_TRANSFER_TO_USER|ユーザーに振り替え待ち
|SUCCESS|両替完了

## ユーザーは小額通貨を両替する

`POST https://api.fcoin.com/v2/broker/auth/small_currency_exchange`

### パラメータ・リクエスト
| 属性  |  説明  |
|:------|:------|
|exchange_currency | もらう通貨：FT
|small_currency_amounts | jsonフォーマット：[{"currency":"usdt","amount":"0.4"},{"currency":"btc","amount":"0.0002"}]。その中で、currencyは送る通貨です。amountは両替の数量です（0より大きくなる必要があります。最大18位の精度をサポートします）。
 
### レスポンス
```
{
  "status": "ok",
  "data": null
}
```

## 両替可能な通貨の照会

`GET https://api.fcoin.com/v2/broker/auth/small_currency_exchange/exchangeable_small_currencies/{exchange_currency}`

### パラメータ・リクエスト

exchange_currency: もらう通貨

### レスポンス

```
  {
    "exchange_currency": "ft",                  もらう通貨
    "total_exchange_amount": "0",               もらった通貨の総額
    "exchangeable_small_currencies": [
      {
          "currency": "dash",                   両替待ちの通貨
          "available_amount": "0.00063517",     利用可能な残高
          "valuation": "0.06805309",            利用可能な残高の価値(usdt)
          "exchange_amount": "1.71670037",      もらえる数量（手数料未支払い）　
          "exchangeable": true                  両替可能ですか（Falseの場合、両替する時計算に入れず）
      },
      {
          "currency": "usdt",
          "available_amount": "0.3",
          "valuation": "0.3",
          "exchange_amount": "15",
          "exchangeable": true
      }
    ]
  }

```

## 両替履歴の照会

`GET https://api.fcoin.com/v2/broker/auth/small_currency_exchange`

### パラメータ・リクエスト

| 属性　| タイプ　　|  説明　|
|:------|:------:|:------|
|has_prev |Boolean| 前のページが含まれますか？このパラメータはフロントエンドにより制御され、そのままで返します。
|id |String|最後のページングのID
|page_size |Integer| リクエスト可能な数量（1〜40）、デフォルトは40です。


#### レスポンス

```
    
 {
  "content": [
    {
      "id": "jS6XfuzBhXXhDBtt_dNDqw",                            記録ID
      "exchange_currency": "ft",                                 もらう通貨
      "fees": "0.056006006006006007",                            手数料
      "exchange_amount": "28.003003003003003003",                もらえる数量
      "actual_exchange_amount": "27.946996996996996996",         両替でもらった数量 ＝ もらえる数量 - 手数料　
      "state": "waiting_transfer_to_user",                       両替状態
      "created_at": 1577949932988                                両替時間
    }
  ],
  "current_elements": 1,
  "has_prev": false,
  "has_next": false,
  "next_page_id": "jS6XfuzBhXXhDBtt_dNDqw"
 }

```
   






  
