# 币币账户与资产

## 查询账户资产

```python
import fcoin

api = fcoin.authorize('key', 'secret', timestamp)
api.accounts_balance
```

```javascript
const fcoin = require('fcoin');

let api = fcoin.authorize('key', 'secret', timestamp);
let orders = api.accountsBalance;
```

> 响应结果如下：

```json
{
  "status": 0,
  "data": [
    {
      "currency": "btc",
      "available": "50.0",
      "frozen": "50.0",
      "balance": "100.0"
    }
  ]
}
```

此 API 用于查询用户的资产列表。

### HTTP Request

`GET https://api.fcoin.com/v2/accounts/balance`


## 从我的钱包划转到交易账户
此接口用于从我的钱包将资产划转到交易账户

### HTTP Request
`POST https://api.fcoin.com/v2/assets/accounts/assets-to-spot`

### 请求参数
|参数|默认值|描述|
|:------|:------:|:------|
currency|  |币种名称
amount|  |数量


```
{
  "amount": 1,
  "currency": "btc",

}
```

### 响应结果
```
{
  "data": null,
  "status": "ok"
}
```