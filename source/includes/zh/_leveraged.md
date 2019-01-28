# 杠杆
## 杠杆账户资产转入
此API用于将交易账户或资产账户的资产转入到杠杆账户。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/leveraged/assets/transfer/in`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|amount|无|划转数量|
|currency|无|币种名称：usdt、btc、eth|
|source_account_type|无|资产来源账户类型: exchange: 交易账户; assets: 资产账户|
|target_account_type|无|目标账户类型: leveraged_btcusdt、leveraged_ethusdt、leveraged_eosusdt、leveraged_xrpusdt|

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "exchange",
  "target_account_type": "leveraged_btcusdt"
}
```

### 响应结果
```
{
  "data": null,
  "status": "ok"
}
```

## 杠杆账户资产转出
此API用于将杠杆账户资产转入到交易账户或资产账户。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/leveraged/assets/transfer/out`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|amount|无|划转数量|
|currency|无|币种名称：usdt、btc、eth、eos、xrp|
|source_account_type|无|资产来源账户类型: leveraged_btcusdt、leveraged_ethusdt、leveraged_eosusdt、leveraged_xrpusdt|
|target_account_type|无|目标账户类型: exchange: 交易账户; assets: 资产账户|

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "leveraged_btcusdt",
  "target_account_type": "exchange"
}
```

### 响应结果
```
{
  "data": null,
  "status": "ok"
}
```

## 查询指定杠杆账户的信息

### 请求url

`GET https://api.fcoin.com/v2/broker/leveraged_accounts/account`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|account_type|无|杠杆账户类型:btcusdt/ethusdt         

### 响应结果

```
{
    "status": "ok",
    "data": {
        "open": true,                                    #是否已经开通该类型杠杆账户. true:已开通;false:未开通
        "leveraged_account_type": "btcusdt",             #杠杆账户类型
        "base": "btc",                                   #基准币种
        "quote": "usdt",                                 #计价币种
        "available_base_currency_amount": "1001.00",     #可用的基准币种资产
        "frozen_base_currency_amount": "0",              #冻结的基准币种资产
        "available_quote_currency_amount": "100100",     #可用的计价币种资产
        "frozen_quote_currency_amount": "0",             #冻结的计价币种资产
        "available_base_currency_loan_amount": "2.000",  #可借的基准币种数量
        "available_quote_currency_loan_amount": "300.000",#可借的计价币种数量
        "blow_up_price": null,                           #爆仓价
        "risk_rate": "0.90",                             #爆仓风险率
        "state": "open"                                  #账户状态. close 已关闭;open 已开通-未发生借贷;normal 已借贷-风险率正常;blow_up 已爆仓;overrun 已穿仓", allowableValues = "close,open,normal,blow_up,overrun")
 
    }
}
```


## 查询所有杠杆账户的信息

### 请求url
`GET https://api.fcoin.com/v2/broker/leveraged_accounts`

### 请求参数
无
### 响应结果
```
{
    "status": "ok",
    "data": [
        {
            "open": true,                                    #是否已经开通该类型杠杆账户. true:已开通;false:未开通
            "leveraged_account_type": "btcusdt",             #杠杆账户类型
            "base": "btc",                                   #基准币种
            "quote": "usdt",                                 #计价币种
            "available_base_currency_amount": "1001.00",     #可用的基准币种资产
            "frozen_base_currency_amount": "0",              #冻结的基准币种资产
            "available_quote_currency_amount": "100100",     #可用的计价币种资产
            "frozen_quote_currency_amount": "0",             #冻结的计价币种资产
            "available_base_currency_loan_amount": "2.000",  #可借的基准币种数量
            "available_quote_currency_loan_amount": "300.000",#可借的计价币种数量
            "blow_up_price": null,                           #爆仓价
            "risk_rate": "0.90",                             #爆仓风险率
            "state": "open"                                  #账户状态. close 已关闭;open 已开通-未发生借贷;normal 已借贷-风险率正常;blow_up 已爆仓;overrun 已穿仓"
        }
    ]
}
```