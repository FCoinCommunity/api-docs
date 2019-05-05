# 币币账户与资产


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