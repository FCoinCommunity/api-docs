# OTC
## OTC資産の振込入金
このAPIは、取引口座やマイウォレットの資産をOTC口座に振込入金するために使用されます。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/assets/transfer/in`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|amount|無|振込入金の数量|
|currency|無|通貨：usdt、btc、eth|
|source_account_type|無|送金口座のタイプ: exchange: 取引口座; assets: マイウォレット|
|target_account_type|無|入金先の口座のタイプ: otc:法定通貨口座|

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "exchange",
  "target_account_type": "otc"
}
```

### 応答結果
```
{
  "data": null,
  "status": "ok"
}
```


## OTC資産の振込出金
このAPIは、OTC口座の資産を取引口座やマイウォレットに振込出金するために使用されます。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/assets/transfer/out`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|amount|無|振込出金の数量|
|currency|無|通貨：usdt、btc、eth|
|source_account_type|無|送金口座のタイプ:otc: 法币账户 |
|target_account_type|無|入金先の口座のタイプ: exchange: 取引口座; assets: マイウォレット|

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "otc",
  "target_account_type": "exchange"
}
```

### 応答結果
```
{
  "data": null,
  "status": "ok"
}
```

## OTC注文
### 新しい注文を作る
このAPIは、新しいOTC注文を作るために使用されます。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/suborders`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|amount|無|取引数量|
|delegation_order_id|無|委託注文id|

```
{
  "amount": 1,
  "delegation_order_id": "St5_OiSE5YQiy4lxkQhq8w"
}
```

### 応答結果
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok",
  "data": 4064224473046529
}
```

## OTC注文リストを照会する
このAPIは、OTC注文リストを照会するために使用されます。

### HTTP Request
`GET https://api.fcoin.com/v2/broker/otc/suborders`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|id|無|最後のページングのID|
|page_size|無|各ページの記録回数|
|delegation_order_id|無|委託注文id|
|states|無|注文状態： 
1 confirmed 確認済 2 buyer_appeal 買い手申し立て中 3 seller_appeal 売り手申し立て中 4 paid 買い手が支払い済み 5 released 通貨が転送済み
6 filled 約定済 7 cancelled キャンセル済 8 timeout キャンセル時間切れ 9 system_cancelled システムキャンセル（残高不足）|


### 応答結果
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok",
  "data": {
    "content": [
      {
        "id": "4064224473046529",
        "currency": "usdt",
        "legal_currency": "cny",
        "direction": "buy",
        "type": "normal",
        "price": "7.00",
        "amount": "100.000000",
        "total_price": "700.00",
        "start_at": 1546614090132,
        "created_at": 1546614090135,
        "state": "confirmed"
      },
      {
        "id": "3214937639915529",
        "currency": "usdt",
        "legal_currency": "cny",
        "direction": "buy",
        "type": "normal",
        "price": "7.70",
        "amount": "100.000000",
        "total_price": "770.00",
        "start_at": 1546421969839,
        "created_at": 1546421969842,
        "state": "filled"
      }
    ],
    "current_elements": 2,
    "has_prev": true,
    "has_next": false,
    "next_page_id": 3214937639915529
  }
}
```

## 注文詳細を照会する
このAPIは、注文詳細を照会するために使用されます。

### HTTP Request
`GET https://api.fcoin.com/v2/broker/otc/suborders/{id}`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|id|無|注文id|

### 応答結果
```
{
  "status": "ok",
  "data": {
    "id": "4064224473046529",
    "currency": "usdt",
    "legal_currency": "cny",
    "direction": "buy",
    "type": "normal",
    "price": "7.00",
    "amount": "100.000000",
    "payment_method": "wechat_pay",
    "total_price": "700.00",
    "start_at": 1546614090132,
    "paid_at": 1546614126421,
    "finished_at": 1546614470679,
    "created_at": 1546614090135,
    "release_begin_at": 1546614294160,
    "buyer_user_id": "hXD7rALO-p899HLEG6OGCA",
    "seller_user_id": "U_g-dtTE_B8Q2eQYa4a8Bw",
    "state": "filled",
    "first_name": "Tim",
    "last_name": "im"
  }
}
```

## 注文支払いの確認
このAPIは、注文の支払いを確認するために使用されます。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/suborders/{id}/pay_confirm`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|id|無|注文id|
|payment_method|無|支払い方法： 
1 alipay Alipay
2 wechat_pay Wechat Pay
3 bank_card_pay 銀行カード


### 応答結果
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok"
}
```

## 買い手の注文キャンセル
このAPIは、買い手が注文をキャンセルするために使用されます。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/suborders/{id}/cancel`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|id|無|注文id|

### 応答結果
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok"
}
```

## 売り手の支払い方法の確認
このAPIは、注文した後、売り手の支払い方法を確認するために使用されます。

### HTTP Request
`GET https://api.fcoin.com/v2/broker/otc/suborders/{id}/payments`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|id|無|注文id|

### 応答結果
```
{
  "status": "ok",
  "data": [
    {
      "payment_method": "alipay",
      "first_name": "Tim",
      "last_name": "im",
      "bank_name": null,
      "bank_branch_name": null,
      "account": "zhifubao",
      "account_image_key": "551c9c2d6bad417fb4aa171bcc6b861d.jpg",
      "account_image_read_url": "https://s3.ap-northeast-1.amazonaws.com/local-s3.bitdict.com/kyc/551c9c2d6bad417fb4aa171bcc6b861d.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20190115T113640Z&X-Amz-SignedHeaders=host&X-Amz-Expires=7200&X-Amz-Credential=AKIAIZ37IMEFO7JZRBOQ%2F20190115%2Fap-northeast-1%2Fs3%2Faws4_request&X-Amz-Signature=26788f3972b7a732b232cc08a54cd7afa038da0a219bf20076267ec70037bf3c"
    },
    {
      "payment_method": "wechat_pay",
      "first_name": "Tim",
      "last_name": "im",
      "bank_name": null,
      "bank_branch_name": null,
      "account": "weixina",
      "account_image_key": "e5b20c5af24232f11ec4ce187f7c6998.jpg",
      "account_image_read_url": "https://s3.ap-northeast-1.amazonaws.com/local-s3.bitdict.com/kyc/e5b20c5af24232f11ec4ce187f7c6998.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20190115T113640Z&X-Amz-SignedHeaders=host&X-Amz-Expires=7200&X-Amz-Credential=AKIAIZ37IMEFO7JZRBOQ%2F20190115%2Fap-northeast-1%2Fs3%2Faws4_request&X-Amz-Signature=8d9e599cf672d3cc495407d2c9538e3b4fb7275aee7201b48b8ba48c65f6fa68"
    }
  ]
}
```

## OTC口座情報の獲得

### HTTP请求

`GET https://api.fcoin.com/v2/broker/otc/users`

### リクエスト・パラメータ
无

### 応答結果

```
{
    "status": "ok",
    "data": {
        "user_id": "Hff9R282Ty8wDa5Xb8AaLg",                #ユーザーID
        "nickname": "isfulei",                              #ニックネーム
        "merchant": true,                                   #OTCメーカーですか？
        "margin_amount": "1.00000000",                      #証拠金の数量
        "total_transaction": 0,                             #約定済の注文総数
        "recent_transaction": 0,                            #最近約定済の注文数
        "total_release_time": "0",                          #合計転送時間（分）
        "average_release_time": "0",                        #平均転送時間（分）
        "recent_success_transaction_ratio": "0",            #最近の成約率
        "register_at": 1,                                   #登録時間
        "remark": "备注",                                    #メモ
        "bind_email": true,                                 #メールアドレスの紐付けを設定しますか？
        "bind_phone": true,                                 #携帯の紐付けを設定しますか？
        "kyc": true,                                        #KYC認証しますか？
        "enable_otc": true,                                 #OTCユーザーでありますか？
        "set_payment_method": false,                        #支払い方法を設定しましたか？
        "set_assets_password": false,                       #資金パスワードを設定しましたか？
        "binding_google_auth": false,                       #2段階認証を設定しましたか？
        "phone": "86-13000000000",                          #電話番号
        "first_name": fu,                                   #姓
        "last_name": lei,                                   #名
        "email": "mock_user@fcoin.com"                      #メール
    }
```

##  OTC口座資金総額の照会 

### リクエスト

```GET https://api.fcoin.com/v2/broker/otc/users/me/balances```

### リクエスト・パラメータ

無

### 応答結果

```
{
    "status": "ok",
    "data": {
        "summary": "0.00000000",
        "balances": [
            {
                "currency": "btc",                 # 通貨
                "available_amount": 20.3202023,    # 利用可能額
                "frozen_amount": 32093.3223        # 凍結中の数量
            },
            {
                "currency": "eth",
                "available_amount": 20.3202023,
                "frozen_amount": 32093.3223
            }
        ]
    }
}            
```

## OTC口座の指定された通貨の資金照会

### リクエスト・URL

`GET https://api.fcoin.com/v2/broker/otc/users/me/balance`

###  リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|currency|無|通貨（例）：btc/eth/usdt

### 応答結果

```
{
    "status": "ok",
    "data": {
        "summary": "0.00000000",                  # btcに換算すると
        "balances": [
            {
                "currency": "btc",               # 通貨
                "available_amount": 20.3202023,  # 利用可能額
                "frozen_amount": 32093.3223      # 凍結中の数量
            }
        ]
    }
}             
```