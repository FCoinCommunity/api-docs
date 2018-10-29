# Accounts and assets

## Query account assets

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

> The response results are as follows：

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

This API is used to query a user's list of assets.

### HTTP Request

`GET https://api.fcoin.com/v2/accounts/balance`
