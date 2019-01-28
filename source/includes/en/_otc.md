# OTC
## OTC Asset transfer
This API is used to transfer the assets in trading account or financial account to OTC account.

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/assets/transfer/in`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|amount|non|Transfer amount|
|currency|non|Currencies：usdt、btc、eth|
|source_account_type|Non |Asset resource type:|exchange: trading account; assets: financial account

|target_account_type|Non|Target account type: otc: OTC account|

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "exchange",
  "target_account_type": "otc"
}
```

### RSRES
```
{
  "data": null,
  "status": "ok"
}
```


## OTC asset transfer out 
This API is used to transfer the assets in OTC account out to the trading account or financial account.

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/assets/transfer/out`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|amount|Non|Transfer amount |
|currency|Non|Currencies：usdt、btc、eth:otc: 
|source_account_type|Non |Asset resource type:| OTC account

|target_account_type|Non|Target account type:exchange: trading account; assets: financial account

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "otc",
  "target_account_type": "exchange"
}
```

### RSRES
```
{
  "data": null,
  "status": "ok"
}
```

## OTC order 
### Create a new order(
This API is used to create a new OTC order.

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/suborders`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|amount|Non|Transaction amount|
|delegation_order_id|Non|Limit order id|

```
{
  "amount": 1,
  "delegation_order_id": "St5_OiSE5YQiy4lxkQhq8w"
}
```

### RSRES
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok",
  "data": 4064224473046529
}
```

## query OTC order list
This API is used to query the order list. 

### HTTP Request
`GET https://api.fcoin.com/v2/broker/otc/suborders`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|id|Non|The last paging id|
|page_size|Non|Record number of each page|
|delegation_order_id|Non|Limit order id|
|states|Non|Order state. 1 confirmed;2 buyer_appeal;3 seller_appeal;4 paid;5 released;6 filled;7 cancelled; 8 timeout; 9 system cancelled (insufficient balance)|


### RSRES
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

## Query order detail
This API is used to query the order list. 

### HTTP Request
`GET https://api.fcoin.com/v2/broker/otc/suborders/{id}`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|id|Non|Order id|

### RSRES
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

## Payment confirmation
This API is used to confirm the payment.

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/suborders/{id}/pay_confirm`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|id|Non|Order id|
|payment_method|Non|Payment method 1 Alipay; 2 WeChat_pay; 3 bank_card_pay


### RSRES
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok"
}
```

## Buyer cancel the order
This API is used by buyer to cancel the order.

### HTTP Request
`POST https://api.fcoin.com/v2/broker/otc/suborders/{id}/cancel`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|id|Non|order id|

### RSRES
```
{
  "err_code": "",
  "err_msg": "",
  "status": "ok"
}
```

## Check payment collecting method of the seller.
This API is used to check the payment collecting method of seller after order being placed.

### HTTP Request
`GET https://api.fcoin.com/v2/broker/otc/suborders/{id}/payments`


### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|id|Non|order id|

### RSRES
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

## Obtain OTC account information

### HTTP request

`GET https://api.fcoin.com/v2/broker/otc/users`

### Required parameter
Non

### RSRES

```
{
    "status": "ok",
    "data": {
        "user_id": "Hff9R282Ty8wDa5Xb8AaLg",                #user ID
        "nickname": "isfulei",                              #nickname 
        "merchant": true,                                   #are you OTC merchant 
        "margin_amount": "1.00000000",                      # margin amount 
        "total_transaction": 0,                             #total transaction volume
        "recent_transaction": 0,                            #recent transaction amount
        "total_release_time": "0",                          #total release time(minutes)
        "average_release_time": "0",                        #average release time(minutes)
        "recent_success_transaction_ratio": "0",            #recent completion rate
        "register_at": 1,                                   #register date
        "remark": "remark",                                    #remark
        "bind_email": true,                                 #has bound email address yet
        "bind_phone": true,                                 #has bound phone number yet
        "kyc": true,                                 #has completed KYC authentication yet
        "enable_otc": true,                                 # are you OTC account user
        "set_payment_method": false,               #has set payment collecting method yet
        "set_assets_password": false,                       #has set asset password yet
        "binding_google_auth": false,                       #has bound 2FA yet
        "phone": "86-13000000000",                          #phone number
        "first_name": fu,                                   #first name
        "last_name": lei,                                   #last name
        "email": "mock_user@fcoin.com"                      #email
    }
```

##  Check the balance of all currencies in oTC Account 

### Reqest

```GET https://api.fcoin.com/v2/broker/otc/users/me/balances```

### Required parameter

Non

### RSRES

```
{
    "status": "ok",
    "data": {
        "summary": "0.00000000",
        "balances": [
            {
                "currency": "btc",                 # currency name
                "available_amount": 20.3202023,    # available amount
                "frozen_amount": 32093.3223        # frozen amount
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

## Obtain the balance of certain currencies in OTC account 

### requesturl

`GET https://api.fcoin.com/v2/broker/otc/users/me/balance`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|currency|Non|currencies，such as：btc/eth/usdt

### RSRES type

```
{
    "status": "ok",
    "data": {
        "summary": "0.00000000",                  # equivalent to _btc
        "balances": [
            {
                "currency": "btc",               # currency name
                "available_amount": 20.3202023,  # available amount
                "frozen_amount": 32093.3223      # frozen amount
            }
        ]
    }
}             
```