# Margin
## Transfer to margin account 
This API is used to transfer the assets in trading account or financial account to margin account.

### HTTP Request
`POST https://api.fcoin.com/v2/broker/leveraged/assets/transfer/in`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|amount|non|Transfer amount|
|currency|Non|Currencies：usdt、btc、eth|
|source_account_type|Non|Asset resource type:|exchange: trading account; assets: financial account
|target_account_type|Non|Target account type:leveraged_btcusdt、leveraged_ethusdt、leveraged_eosusdt、leveraged_xrpusdt|

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "exchange",
  "target_account_type": "leveraged_btcusdt"
}
```

###  RSRES
```
{
  "data": null,
  "status": "ok"
}
```

##  margin account asset transfer out 
This API is used to transfer the assets in margin account out to the trading account or financial account.

### HTTP Request
`POST https://api.fcoin.com/v2/broker/leveraged/assets/transfer/out`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|amountNon|Transfer amount |
|currency||Non|Currencies：usdt、btc、eth、eos、xrp|
|source_account_type |Non|Asset resource type: leveraged_btcusdt、leveraged_ethusdt、leveraged_eosusdt、leveraged_xrpusdt|
|target_account_type|Non|Target account type: exchange: trading account; assets:financial account|

```
{
  "amount": 1,
  "currency": "btc",
  "source_account_type": "leveraged_btcusdt",
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

## Obtain the balance of certain currencies in margin account 

### request url

`GET https://api.fcoin.com/v2/broker/leveraged_accounts/account`

### Required parameter
|Parameter|Default|Description|
|:------|:------:|:------|
|account_type|Non|margin account type:btcusdt/ethusdt         

### RSRES type

```
{
    "status": "ok",
    "data": {
        "open": true,                                    #has opened the margin account of this type yet? true;false:
        "leveraged_account_type": "btcusdt",             #margin account type
        "base": "btc",                                   #base currency
        "quote": "usdt",                                 #dominated currency
        "available_base_currency_amount": "1001.00",     #available asset of base currencies 
        "frozen_base_currency_amount": "0",              #frozen asset of base currencies
        "available_quote_currency_amount": "100100",     #available asset of dominated currencies
        "frozen_quote_currency_amount": "0",             #frozen asset of dominated currencies
        "available_base_currency_loan_amount": "2.000",  #base currencies amount you can borrow         
        "available_quote_currency_loan_amount": "300.000",#dominated currencies amount you can borrow 
        "blow_up_price": null,                           #blow-up price
        "risk_rate": "0.90",                             #blow-up risk ratios
        "state": "open"                                  #account state. close ;open-has borrow any loans yet ;normal has borrowed-normal risk ratio; blow_up ;overrun ", allowableValues = "close,open,normal,blow_up,overrun")
 
    }
}
```


## btain the balance of all currencies in margin account 

### request url
`GET https://api.fcoin.com/v2/broker/leveraged_accounts`

### Required parameter
Non
### RSRES
```
{
    "status": "ok",
    "data": [
        {
            "open": true,                                    #has opened the margin account of this type yet? true;false:
            "leveraged_account_type": "btcusdt",             #margin account type
            "base": "btc",                                   #base currency
            "quote": "usdt",                                 #dominated currency
            "available_base_currency_amount": "1001.00",     #available asset of base currencies 

            "frozen_base_currency_amount": "0",              #frozen asset of base currencies 

            "available_quote_currency_amount": "100100",    #available asset of dominated currencies

            "frozen_quote_currency_amount": "0",            #frozen asset of dominated currencies

            "available_base_currency_loan_amount": "2.000",  #base currencies amount you can borrow         

            "available_quote_currency_loan_amount": "300.000",#dominated currencies amount you can borrow 

            "blow_up_price": null,                           #blow-up price
            "risk_rate": "0.90",                             #blow-up risk
            "state": "open"                                  #account state. close; open-has borrow any loans yet ;normal has borrowed-normal risk ratio; blow_up ;overrun ", 
        }
    ]
}
```