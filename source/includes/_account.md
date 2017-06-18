# Account

## Create an account.
### Base URL: 

`/accounts`

This account creates a hot wallet for user. Using it user can make a deposit or
withdrawals. Reminder, an *api_key* is supplied with every call.

### HTTP Request

`POST http://localhost:8000/accounts?project_key={{project_key}}`

```shell
curl "http://localhost:8000/accounts?project_key={{project_key}}"
  -H "PROJECTKEY: 72tyrha545ccaa7d1c483782783914911fe52df3"
```



```json
{
  "public_address": "0x1000000000000000000000000000000000000000",
  "secret_key": "fdhfiuqwhr78fuhsdf7gyartyhdjgyfdsjhfdghds",
  "confirmation_level": 6,
  "default_callback": "hell.comm",
  "default_deposit_behavior": {"recipients": [
    {
      "receival_address": "0x1000000000000000000000000000000000000000",
      "withdrawal_percent": 52
    },
    {
      "receival_address": "0x1220000000000000000000000000000000000000",
      "withdrawal_percent": 48
    }
    ]}
}
```


### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
callback_url | "" | 
confirmation_level | 1 - minimum , 10 - maximum | 
recipient_list |[]|Format: [{address: percent}, {address: percent}]. Percents must sum to 100.

<aside class="notice">
  If a param is not supplied, and there is no project default, an error is returned. If a recipient list is supplied and does not sum to 100, an error is returned.
</aside>

## Get accounts

This endpoint returns a list of all accounts associated with a project along with their balances (pending, confirmed, transferred).

### HTTP Request

`POST http://localhost:8000/accounts?project_key={{project_key}}`

```shell
curl "http://localhost:8000/accounts?project_key={{project_key}}"
  -H "PROJECTKEY: 72tyrha545ccaa7d1c483782783914911fe52df3"
```

```json
{
  [{public_address: "0x1000000000000000000000000000000000000000"
     {pending_balance: 1231231, 
      confirmed_balance: 211231, 
      withdraw: 213121}}
  ]
}
```

## Get public_address
### Base URL: 

### HTTP Request

`GET http://localhost:8000/accounts/?public_address="0x1000000000000000000000000000000000000000"?project_key={{project_key}}`

```shell
curl "http://localhost:8000/accounts/?public_address=""0x1000000000000000000000000000000000000000""?project_key={{project_key}}"
  -H "PROJECTKEY: 72tyrha545ccaa7d1c483782783914911fe52df3"
```

```json
{
   "pending_balance": 1000000 # in weis
   "confirmed_balance": 10000, 
   "withdraw": 1232132131
}
```