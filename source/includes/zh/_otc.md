# OTC
## OTC 资产转入
此API用于将交易账户或资产账户的资产转入到OTC账户。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/assets/transfer/in`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|amount|无|划转数量|
|currency|无|币种名称：usdt、btc、eth|
|source_account_type|无|资产来源账户类型: exchange: 交易账户; assets: 资产账户|
|target_account_type|无|目标账户类型: otc: 法币账户|

### 请求示例：
```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "exchange",
  "target_account_type": "otc"
}
```

### 响应结果
```
{
  "data": null,
  "status": "ok"
}
```


## OTC资产转出
此API用于将OTC账户资产转入到交易账户或资产账户。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/assets/transfer/out`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|amount|无|划转数量|
|currency|无|币种名称：usdt、btc、eth|
|source_account_type|无|资产来源账户类型:otc: 法币账户 |
|target_account_type|无|目标账户类型: exchange: 交易账户; assets: 资产账户|

### 请求示例：
```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "otc",
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

## OTC订单
### 创建新的订单
此API用于创建一个新的OTC订单。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/suborders`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|amount|无|交易数量|
|delegation_order_id|无|委托单id|

### 请求示例：

```
{
  "amount": 1,
  "delegation_order_id": "St5_OiSE5YQiy4lxkQhq8w"
}
```

### 响应结果
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok",
  "data": 4064224473046529
}
```

## 查询OTC订单列表
此API用于查询订单列表。

### HTTP Request
`GET https://api.fcoin.com/v2/broker/otc/suborders`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|id|无|最后一次分页的id|
|page_size|无|每页记录数|
|delegation_order_id|无|委托单id|
|states|无|订单状态。 1 confirmed 已确认;2 buyer_appeal 买方申诉中;3 seller_appeal 卖方申诉中;4 paid 买家已付款;5 released 卖家已放币;6 filled 已成交;7 cancelled 已撤销;8 timeout 超时撤销;9 system_cancelled 系统撤销（余额不足）|


### 响应结果
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

## 查询订单详情
此API用于查询订单详情。

### HTTP Request
`GET https://api.fcoin.com/v2/broker/otc/suborders/{id}`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|id|无|订单id|

### 响应结果
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

## 订单确认支付
此API用于确认支付。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/suborders/{id}/pay_confirm`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|id|无|订单id|
|payment_method|无|支付方式。 1 alipay 支付宝;2 wechat_pay 微信;3 bank_card_pay 银行卡


### 响应结果
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok"
}
```

## 买家取消订单
此API用于买家取消订单。

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/suborders/{id}/cancel`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|id|无|订单id|

### 响应结果
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok"
}
```

## 查询卖家支付方式
此API用于下订单后，查询卖家的支付方式。

### HTTP Request
`GET https://api.fcoin.com/v2/broker/otc/suborders/{id}/payments`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|id|无|订单id|

### 响应结果
```
{
  "status": "ok",
  "data": [
    {
      "payment_method": "alipay",
      "first_name": "first_name",
      "last_name": "last_name",
      "bank_name": null,
      "bank_branch_name": null,
      "account": "alipay_account",
      "account_image_key": "551c9c2d6bad417fb4aa171bcc6b861d.jpg",
      "account_image_read_url": "***"
    },
    {
      "payment_method": "wechat_pay",
      "first_name": "first_name",
      "last_name": "last_name",
      "bank_name": null,
      "bank_branch_name": null,
      "account": "wechat_pay_account",
      "account_image_key": "e5b20c5af24232f11ec4ce187f7c6998.jpg",
      "account_image_read_url": "***"
    },
    {
      "payment_method": "bank_card_pay",
      "first_name": "first_name",
      "last_name": "last_name",
      "bank_name": "A银行",
      "bank_branch_name": "A银行XX支行",
      "account": "bank_card_pay_account",
      "account_image_key": "e5b20c5af24232f11ec4ce187f7c6998.jpg",
      "account_image_read_url": "***"
    }
  ]
}
```

## 获取OTC账户信息

### HTTP请求

`GET https://api.fcoin.com/v2/broker/otc/users`

### 请求参数
无

### 响应结果

```
{
    "status": "ok",
    "data": {
        "user_id": "Hff9R282Ty8wDa5Xb8AaLg",                #用户ID
        "nickname": "isfulei",                              #昵称
        "merchant": true,                                   #是否是商家
        "margin_amount": "1.00000000",                      #保证金数量
        "total_transaction": 0,                             #总成交数
        "recent_transaction": 0,                            #近期成交数
        "total_release_time": "0",                          #总放行时间(分钟)
        "average_release_time": "0",                        #平均放行时间(分钟)
        "recent_success_transaction_ratio": "0",            #近期成交率
        "register_at": 1,                                   #注册时间
        "remark": "备注",                                    #备注
        "bind_email": true,                                 #是否绑定邮箱
        "bind_phone": true,                                 #是否绑定手机
        "kyc": true,                                        #是否KYC
        "enable_otc": true,                                 #是否是OTC用户
        "set_payment_method": false,                        #是否设置了支付方式
        "set_assets_password": false,                       #是否设置了资金密码
        "binding_google_auth": false,                       #是否绑定了GA
        "phone": "86-13000000000",                          #联系电话
        "first_name": fu,                                   #first name
        "last_name": lei,                                   #last name
        "email": "mock_user@fcoin.com"                      #邮箱
    }
```

##  查询OTC账户所有币种余额 

### 请求

```GET https://api.fcoin.com/v2/broker/otc/users/me/balances```

### 请求参数

无

### 响应结果

```
{
    "status": "ok",
    "data": {
        "summary": "0.00000000",                   # 折合btc的数量
        "balances": [
            {
                "currency": "btc",                 # 币种名称
                "available_amount": 20.3202023,    # 可用数量
                "frozen_amount": 32093.3223        # 冻结数量
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

## 获取OTC账户指定币种余额

### 请求url

`GET https://api.fcoin.com/v2/broker/otc/users/me/balance`

###  请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|currency|无|币种名称，如：btc/eth/usdt

### 响应结果

```
{
    "status": "ok",
    "data": {
        "summary": "0.00000000",                  # 折合btc的数量
        "balances": [
            {
                "currency": "btc",               # 币种名称
                "available_amount": 20.3202023,  # 可用数量
                "frozen_amount": 32093.3223      # 冻结数量
            }
        ]
    }
}             
```

## OTC 委托单
- 委托单状态说明:
 
   属性  |  含义
   ----  |  ----- 
   SUBMITTED|已提交
   CONFIRMED|已确认 
   PARTIAL_FILLED|部分成交
   PARTIAL_CANCELED|部分成交(已撤单)
   FILLED|完全成交
   CANCELED|已撤销
   SYSTEM_CANCELED|系统撤销(余额不足)

### 创建otc委托单

   `POST https://api.fcoin.com/v2/broker/otc/delegation_orders/submit`
    
### 请求参数:
```
    {
            "amount": 0,                    总数量
            "currency": "string",           支持OTC的币种名称 BTC; ETH; USDT
            "direction": "string",          买卖方向：  buy 买; sell 卖
            "legal_currency": "string",     法币币种   cny 人民币
            "max_limit": 0,                 最大买/卖金额
            "min_limit": 0,                 最小买/卖金额
            "payment_methods": "string"     支付方式. alipay 支付宝; wechat_pay 微信; bank_card_pay 银行卡 添加多种支付方式用","隔开 
            "price": 0,                     价格
            "remark": "string"              备注
    }
```	
	
### 响应结果   
```	
    {
        "status": "ok",
        "data": "string"                    委托单id
    }
```

## 查询指定委托单 
### HTTP Request
  `GET https://api.fcoin.com/v2/broker/otc/delegation_orders/{delegation_order_id}`

###  请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|delegation_order_id|无|订单ID

### 响应结果
```
    {
     "amount": 0,                       总数量
    "canceled_at": 0,                   委托单被撤销时间
    "currency": "string",               数字货币名称
    "direction": "string",              买卖方向
    "filled_amount": 0,                 已经完成的数量
    "finished_at": 0,                   委托单完成时间
    "id": 0,                            委托单ID
    "legal_currency": "string",         法币币种名称
    "max_limit": 0,                     最大买/卖金额
    "min_limit": 0,                     最小买/卖金额
    "payment_methods": [                支付方式
      "string"
    ],
    "price": 0,                         单价
    "processing_amount": 0,             处理中的数量
    "remark": "string",                 委托单备注
    "state": "string",                  委托单状态
    "total_amount": 0,                  总金额
    "type": "string",                   委托单类型
    "unprocessed_amount": 0             未处理的数量
    }
```
## 撤销otc委托单 
### HTTP Request
  `POST https://api.fcoin.com/v2/broker/otc/delegation_orders/{delegation_order_id}/cancel`

###  请求参数
|参数|默认值|描述|
|:------|:------:|:------|
|delegation_order_id|无|订单ID

## 查询当前用户委托单
### HTTP Request
  `GET https://api.fcoin.com/v2/broker/otc/delegation_orders/me`

### 请求参数

|参数|默认值|描述|
|:------|:------:|:------|
|has_prev|无|是否包含前一页，该参数由前端控制，原样返回
|id|无|最后一次分页的ID
|page_size integer|无| 请求数量，1-40
|states|无| 委托单状态 1 trading 交易中;2 filled 已成交;3 partial_filled 部分成交;4 canceled 已撤销

### 响应结果

```
    
    {
        "content": [
        {
            "amount": 0,                        总数量
            "created_at": 0,                    创建时间
            "currency": "string",               数字货币名称
            "direction": "string",              买卖方向
            "filled_amount": 0,                 已经完成的数量
            "id": 0,                            委托单ID
            "legal_currency": "string",         法币币种名称
            "max_limit": 0,                     最大买/卖金额
            "min_limit": 0,                     最小买/卖金额
            "payment_methods": [                支付方式
            "string"
            ],
            "price": 0,                         单价
            "processing_amount": 0,             处理中的数量
            "state": "string",                  委托单状态 submited 已提交;confirmed 已确认; partial_filled 部分                                     成交;partial_canceled 部分成交（已撤单） filled 完全成交;                                              canceled 已撤销; system_canceled 系统撤销（余额不足）
            "total_amount": 0,                  总金额
            "unprocessed_amount": 0             未处理的数量
            "next_refresh_at"				        下一次刷新时间
            "refresh_at"			   	        刷新时间
        }
        ],
        "current_elements": 0, 查询结果集数量
        "has_next": true,      是否包含下一页
        "has_prev": true,      是否包含前一页，该参数由前端控制，原样返回
        "next_page_id": "string" 下一页查询条件ID
    }

```
## 刷新委托单
`POST https://api.fcoin.com/v2/broker/otc/delegation_orders/{delegation_order_id}/refresh`

### 请求参数

|参数|默认值|描述|
|:------|:------:|:------|
|delegation_order_id|无|委托单ID

### 响应结果
```
{
  "data": null,
  "status": "ok"
}

```