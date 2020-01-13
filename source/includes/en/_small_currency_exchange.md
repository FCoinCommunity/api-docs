# small assets conversion 

- Conversion record state description 
 
 
| Property   |  Meaning |
|:------|:------:|
|WAITING_TRANSFER_TO_SYSTEM|Waiting to be transferred into system account 
|WAITING_REFUND|Refund to users  
|FAIL|Conversion failed
|WAITING_TRANSFER_TO_USER|Waiting to transfer to the user 
|SUCCESS|Conversion succeed 

## User convert the small assets

`POST https://api.fcoin.com/v2/broker/auth/small_currency_exchange`

### Required Parameter  
| Property |  Meaning |
|:------|:------|
|exchange_currency | Convert into,ft
|small_currency_amounts | json format: [{"currency":"usdt","amount":"0.4"},{"currency":"btc","amount":"0.0002"}]，among currency: currency to be converted，amount: convention amount (must be more than 0，18 digital precision are supported max. ) 
 
### Responding result  
```
{
  "status": "ok",
  "data": null
}
```

## Check convertible currency 

`GET https://api.fcoin.com/v2/broker/auth/small_currency_exchange/exchangeable_small_currencies/{exchange_currency}`

### Required Parameter  

exchange_currency: Converted into currency

###  Responding result    

```
　{
  　"exchange_currency": "ft",                  Converted into currency　　
    "total_exchange_amount": "0",               total amount have been coverted
    "exchangeable_small_currencies": [
      {
          "currency": "dash",                   currency to be converted
          "available_amount": "0.00063517",     currency balance 
          "valuation": "0.06805309",            available balance estimated (usdt)
          "exchange_amount": "1.71670037",      converted amount(commission has not been deducted)
          "exchangeable": true                  Can be converted or no(false means can be neglected during conversion)  
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

## Check conversion record

`GET https://api.fcoin.com/v2/broker/auth/small_currency_exchange`

### Required Parameter    

| Property| Type |  Meaning |
|:------|:------:|:------|
|has_prev |Boolean| the Parameter is controlled by the front end weather to include the former page or not, return as it
|id |String|The last paging ID not mandatory 
|page_size |Integer| Request amount(1-40),40 by default


#### Responding result

```
    
 {
  "content": [
    {
      "id": "jS6XfuzBhXXhDBtt_dNDqw",                            record id
      "exchange_currency": "ft",                                 convert into currency
      "fees": "0.056006006006006007",                            fee
      "exchange_amount": "28.003003003003003003",                converted amount
      "actual_exchange_amount": "27.946996996996996996",         actual arrival amount = converted amount - fee
      "state": "waiting_transfer_to_user",                       conversion state
      "created_at": 1577949932988                                conversion time
    }
  ],
  "current_elements": 1,
  "has_prev": false,
  "has_next": false,
  "next_page_id": "jS6XfuzBhXXhDBtt_dNDqw"
 }

```
   






  
