# 币币账户接口


## 查询交易账户的资产列表。

此api用于查询交易账户的资产
### HTTP Request

`GET https://api.fcoin.com/v2/accounts/balance`

```
```



## 查询我的钱包的资产列表。
此api用于查询我的钱包的资产

### HTTP Request

`GET https://api.fcoin.com/v2/assets/accounts/balance`

### 响应结果
```
{
  'status': 0,
  'data': [
    {
      'currency': 'btc',                        币种名称
      'available': '0.000000000000000000',      可用
      'frozen': '0.000000000000000000',         冻结
      'demand_deposit': '0.000000000000000000', 理财资产
      'lock_deposit': '0.000000000000000000',   锁仓资产
      'balance': '0.000000000000000000'         总资产
    },
    {
      'currency': 'eth',
      'available': '0.000000000000000000',
      'frozen': '0.000000000000000000',
      'demand_deposit': '0.000000000000000000',
      'lock_deposit': '0.000000000000000000',
      'balance': '0.000000000000000000'
    },
    ...
}
```



## 从我的钱包划转到交易账户
此接口用于从我的钱包将资产划转到交易账户

### HTTP Request
`POST https://api.fcoin.com/v2/assets/accounts/assets-to-spot`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
currency|  |币种名称
amount|  |数量
### 请求示例：
```
{
  "currency": "btc",
  "amount": 1
}
```
```
```
### 响应结果
```
{
  "data": null,
  "status": "ok"
}
```

## 从交易账户划转到我的钱包
此接口用于从交易账户将资产划转到我的钱包

### HTTP Request
`POST https://api.fcoin.com/v2/accounts/spot-to-assets`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
currency|  |币种名称
amount|  |数量
### 请求示例：
```
{
  "currency": "btc",
  "amount": 1
}
```
### 响应结果
```
{
  "data": null,
  "status": "ok"
}
```


## 碎币兑换  

- 兑换记录状态说明:
 
 
| 属性   |  含义|
|:------|:------:|
|WAITING_TRANSFER_TO_SYSTEM|待划转到系统账户
|WAITING_REFUND|待退款给用户 
|FAIL|兑换失败
|WAITING_TRANSFER_TO_USER|待打款给用户
|SUCCESS|兑换成功

## 用户兑换碎币

`POST https://api.fcoin.com/v2/broker/auth/small_currency_exchange`

### 请求参数   
| 属性 |  含义 |
|:------|:------|
|exchange_currency | 要兑换成的币种，ft
|small_currency_amounts | json格式: [{"currency":"usdt","amount":"0.4"},{"currency":"btc","amount":"0.0002"}]，其中currency: 待兑换的币种，amount: 要兑换的数量(必须大于0，最大支持18位精度) 
 
### 响应结果  
```
{
  "status": "ok",
  "data": null
}
```

## 查询可兑换的币种 

`GET https://api.fcoin.com/v2/broker/auth/small_currency_exchange/exchangeable_small_currencies/{exchange_currency}`

### 请求参数

exchange_currency: 要兑换成的币种

### 响应结果  

```
  {
    "exchange_currency": "ft",                  要兑换成的币种
    "total_exchange_amount": "0",               已经兑换所得总量
    "exchangeable_small_currencies": [
      {
          "currency": "dash",                   待兑换的币种
          "available_amount": "0.00063517",     币种可用余额
          "valuation": "0.06805309",            可用余额估值(usdt)
          "exchange_amount": "1.71670037",      兑换可得数量(未扣除手续费)
          "exchangeable": true                  是否可兑换(如果为false,兑换时将忽略不计)  
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

## 查询兑换记录 

`GET https://api.fcoin.com/v2/broker/auth/small_currency_exchange`

### 请求参数  

| 属性| 类型 |  含义 |
|:------|:------:|:------|
|has_prev |Boolean| 是否包含前一页，该参数由前端控制，原样返回
|id |String|最后一次分页的ID,非必传
|page_size |Integer| 请求数量(1-40),默认为40


#### 响应结果

```
    
 {
  "content": [
    {
      "id": "jS6XfuzBhXXhDBtt_dNDqw",                            记录id
      "exchange_currency": "ft",                                 兑换成的币种
      "fees": "0.056006006006006007",                            手续费
      "exchange_amount": "28.003003003003003003",                兑换应得数量
      "actual_exchange_amount": "27.946996996996996996",         兑换实际所得数量=兑换应得数量-手续费
      "state": "waiting_transfer_to_user",                       兑换状态
      "created_at": 1577949932988                                兑换时间
    }
  ],
  "current_elements": 1,
  "has_prev": false,
  "has_next": false,
  "next_page_id": "jS6XfuzBhXXhDBtt_dNDqw"
 }

```
   






  
