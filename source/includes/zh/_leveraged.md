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
### 请求示例：
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
### 请求示例：
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

## 杠杆借贷和还款

## 下借款单

`POST https://api.fcoin.com/v2/broker/leveraged/loans`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|account_type|无|杠杆账户类型:btcusdt/ethusdt 
|amount|无|借款数量
|currency|无|借款币种名称
|loan_type|无|借款类型. normal 正常借款（指定借款数量）; all 全部（把剩余资产全部用来借款）

### 响应结果
```
        {
        "amount": 0,                借款数量
        "created_at": 0,            账单生成时间
        "currency": "string",       借款币种名称
        "finished_at": 0,           还款完成时间
        "interest": 0,              总利息
        "interest_rate": 0,         借款利率
        "interest_start_at": 0,     计息开始时间
        "last_repayment_at": 0,     最后一次还款时间
        "loan_bill_id": 0,          借贷账单ID
        "next_interest_at": 0,      下一次计息时间
        "state": "string",          账单状态. submitted 已提交; 2 confirmed 已确认; 5 finished 还款完成
        "unpaid_amount": 0,         未还款数量
        "unpaid_interest": 0,       未还利息
        "unpaid_total_amount": 0    待还款总数量 (未还款数量+未还利息)
        }

```


## 自定义查询杠杆借贷记录  

`GET https://api.fcoin.com/v2/broker/leveraged/loans`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|has_prev|无|是否包含前一页，该参数由前端控制，原样返回
|id|无| 最后一次分页的ID
|page_size|无| 请求数量，1-40
|account_type|无| 借款账户类型 BTCUSDT ETHUSDT 
|skip_finish|false| 是否忽略已完成状态的借款单，默认不忽略

### 响应结果
  
  ```
        {
        "content": [
            {
            "amount": 0,                借款数量
            "created_at": 0,            账单生成时间
            "currency": "string",       借款币种名称
            "finished_at": 0,           还款完成时间
            "interest": 0,              总利息
            "interest_rate": 0,         借款利率
            "interest_start_at": 0,     计息开始时间
            "last_repayment_at": 0,     最后一次还款时间
            "loan_bill_id": 0,          借贷账单ID
            "next_interest_at": 0,      下一次计息时间
            "state": "string",          账单状态. submitted 已提交; 2 confirmed 已确认; 5 finished 还款完成
            "unpaid_amount": 0,         未还款数量
            "unpaid_interest": 0,       未还利息
            "unpaid_total_amount": 0    待还款总数量 (未还款数量+未还利息)
        }
        ],
        "current_elements": 0,          查询结果集数量
        "has_next": true,               是否包含下一页
        "has_prev": true,               是否包含前一页，该参数由前端控制，原样返回
        "next_page_id": "string"        下一页查询条件ID
    }
  ```  

## 查询指定杠杆借贷详情

`GET https://api.fcoin.com/v2/broker/leveraged/loans/{leveraged_loan_id}`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|leveraged_loan_id|无|借贷账单ID
### 响应结果
```

     {
                "amount": 0,                借款数量
                "created_at": 0,            账单生成时间
                "currency": "string",       借款币种名称
                "finished_at": 0,           还款完成时间
                "interest": 0,              总利息
                "interest_rate": 0,         借款利率
                "interest_start_at": 0,     计息开始时间
                "last_repayment_at": 0,     最后一次还款时间
                "loan_bill_id": 0,          借贷账单ID
                "next_interest_at": 0,      下一次计息时间
                "state": "string",          账单状态. submitted 已提交; 2 confirmed 已确认; 5 finished 还款完成
                "unpaid_amount": 0,         未还款数量
                "unpaid_interest": 0,       未还利息
                "unpaid_total_amount": 0    待还款总数量 (未还款数量+未还利息)
        }
```


## 杠杆借款单还款
`POST https://api.fcoin.com/v2/broker/leveraged/repayments/{loan_bill_id}`
### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|loan_bill_id|无|借款单编号
```
  body
  {
   "amount": 0  还款数量
  } 
```
### 响应结果
  还款单ID