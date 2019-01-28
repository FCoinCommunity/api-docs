# レバレッジ
## レバレッジ口座へ資産の振込入金
このAPIは、取引口座或いはマイウォレットの資産をレバレッジ口座に振込入金するために使用されます。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/leveraged/assets/transfer/in`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|amount|無|振込入金の数量|
|currency|無|通貨：usdt、btc、eth|
|source_account_type|無|送金口座のタイプ: exchange: 取引口座; assets: マイウォレット |
|target_account_type|無|入金先の口座のタイプ: leveraged_btcusdt、leveraged_ethusdt、leveraged_eosusdt、leveraged_xrpusdt|

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "exchange",
  "target_account_type": "leveraged_btcusdt"
}
```

### 応答結果
```
{
  "data": null,
  "status": "ok"
}
```

## レバレッジ口座から資産の振込出金
このAPIは、レバレッジ口座から取引口座或いはマイウォレットに資産を振込出金するために使用されます。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/leveraged/assets/transfer/out`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|amount|無|振込出金の数量|
|currency|無|通貨：usdt、btc、eth、eos、xrp|
|source_account_type|無|資産の元口座のタイプ: leveraged_btcusdt、leveraged_ethusdt、leveraged_eosusdt、leveraged_xrpusdt|
|target_account_type|無|出金先の口座のタイプ:: exchange: 取引口座; assets: マイウォレット|

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "leveraged_btcusdt",
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

## 指定されたレバレッジ口座の情報を照会する

### リクエスト・URL

`GET https://api.fcoin.com/v2/broker/leveraged_accounts/account`

### リクエスト・パラメータ
|ペラメータ|デフォルト値|説明|
|:------|:------:|:------|
|account_type|無|レバレッジ口座のタイプ:btcusdt/ethusdt         

### 応答結果

```
{
    "status": "ok",
    "data": {
       "open": true,                                    #本タイプのレバレッジ口座を開設しましたか？ true:はい;false:いいえ
        "leveraged_account_type": "btcusdt",             #レバレッジ口座のタイプ
        "base": "btc",                                   #基軸通貨
        "quote": "usdt",                                 #評価通貨
        "available_base_currency_amount": "1001.00",     #利用可能の基軸通貨資産
        "frozen_base_currency_amount": "0",              #凍結中の基軸通貨資産
        "available_quote_currency_amount": "100100",     #利用可能の評価通貨資産
        "frozen_quote_currency_amount": "0",             #凍結中の評価通貨資産
        "available_base_currency_loan_amount": "2.000",  #貸借可能の基軸通貨資産        
　　　　　"available_quote_currency_loan_amount": "300.000",#貸借可能の評価通貨数量
        "blow_up_price": null,                           #清算価格
        "risk_rate": "0.90",                             #ロスカットリスク率
        "state": "open"                                  #口座状態. close クローズ;open オープン-貸借無し;normal 貸借あり-ノーマルなリスク率;blow_up ロスカット;overrun 追証", allowableValues = "close,open,normal,blow_up,overrun")
 
    }
}
```


## 全てのレバレッジ口座の情報を照会する

### リクエスト・URL
`GET https://api.fcoin.com/v2/broker/leveraged_accounts`

### リクエスト・パラメータ
无
### 応答結果
```
{
    "status": "ok",
    "data": [
        {
       "open": true,                                    #本タイプのレバレッジ口座を開設しましたか？ true:はい;false:いいえ
        "leveraged_account_type": "btcusdt",             #レバレッジ口座のタイプ
        "base": "btc",                                   #基軸通貨
        "quote": "usdt",                                 #評価通貨
        "available_base_currency_amount": "1001.00",     #利用可能の基軸通貨資産
        "frozen_base_currency_amount": "0",              #凍結中の基軸通貨資産
        "available_quote_currency_amount": "100100",     #利用可能の評価通貨資産
        "frozen_quote_currency_amount": "0",             #凍結中の評価通貨資産
        "available_base_currency_loan_amount": "2.000",  #貸借可能の基軸通貨資産        
　　　　　"available_quote_currency_loan_amount": "300.000",#貸借可能の評価通貨数量
        "blow_up_price": null,                           #清算価格
        "risk_rate": "0.90",                             #ロスカットリスク率
        "state": "open"                                  #口座状態. close クローズ;open オープン-貸借無し;normal 貸借あり-ノーマルなリスク率;blow_up ロスカット;overrun 追証"
        }
    ]
}
```