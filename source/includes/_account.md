# Account

This is the function model of the API. It provides the ability to receive Ethereum payments and associate them with users of your application.

### Using Accounts in your Application

Typically, you will create one account per user that signs up to your app. You will then display the account's `public_address` to the user. If the user pays the account, a callback will be made to your server and your application logic can proceed knowing the user has paid. 

This flow may change depending on application. You could create one account per transaction, with a user being shown a different public address to complete each transaction they start in your application. This is entirely up to you.

### Base URL

The base route related to all project actions is `https://ethereum-processor.herokuapp.com/accounts`. 

### Fields

Accounts have 5 fields. 

Field | Format | Description
--------- | ------- | -----------
public_address | String, 42 character Ethereum address | The address at which the account receives payments.
secret_withdrawal_key | String, 16 digits | The secret key which you can use to control certain aspects of the account and validate the authenticity of callbacks.
callback_url | String, valid URL | The URL to which a callback is made when the account receives a payment.
confirmation_level | Integer, minimum 1 | The number of confirmations before a received payment is treated as confirmed. 
deposit_behavior | JSON Dictionary | The value of this field determines how ethereum received by accounts is distributed. More on this later.


## Create an Account Using Project Defaults

> Request:

```python
requests.post(
  "https://ethereum-processor.herokuapp.com/accounts",
  headers={
    "PROJECT_KEY": "YOUR_KEY"
  })
```

> Response:

```json
{
  "public_address": "0x68bc4251db49cfe754bc58afeac1ef27e8e29cd7",
  "secret_withdrawal_key": "3694e680b4487e60",
  "confirmation_level": 6,
  "callback": "your_app_server.com",
  "deposit_behavior": {
    "recipients": [
      {
        "receival_address": "0xdeadbeefD68880146dF45379661546AB4aBF6C78",
        "withdrawal_percent": 100.0
      }
    ]
  }
}
```

In most cases, your project should have values set for all three default fields (`default_callback`, `default_confirmation_level`, and `default_deposit_behavior`). If this is the case, you can create a new account without supplying any additional information.

### Making the Request

To create an account based on project defaults, you should make a POST request to `"https://ethereum-processor.herokuapp.com/accounts`. You will need to

1. supply your Project Key in the headers, using the key `PROJECT_KEY`.

<aside class="notice">
  If there is no project default for a field, and you attempt this request, an error is returned.
</aside>

### Understanding the Response Data

In the response to your request, you will see that the callback, confirmation_level, and deposit_behavior fields have been filled with your projects defaults. This is as expected. You may choose to save these values or not.

The `public_address` and `secret_withdrawal_key` are crucial and must be saved. The `public_address` is the address you will show your user in order for them to pay you. The `secret_withdrawal_key` will be used to validate the authenticity of callbacks and to control certain aspects of the account.


## Create an Account with Custom Settings

> Request:

```python
requests.post(
  "https://ethereum-processor.herokuapp.com/accounts",
  json={
    "confirmation_level": 9,
    "callback": "your_custom_app_server.com",
    "deposit_behavior": {"recipients": [
        {
          "receival_address": "0xdeadbeef10000000000000000000000000000000",
          "withdrawal_percent": 50.0
        },
        {
          "receival_address": "0xdeadbeef20000000000000000000000000000000",
          "withdrawal_percent": 50.0
        }
      ]
    }
  }
  headers={
    "PROJECT_KEY": "YOUR_KEY"
  })
```

> Response:

```json
{
  "public_address": "0x68bc4251db49cfe754bc58afeac1ef27e8e29cd7",
  "secret_withdrawal_key": "3786e680b448yyts",
  "confirmation_level": 9,
  "callback": "your_custom_app_server.com",
  "deposit_behavior": {
    "recipients": [
      {
        "receival_address": "0xdeadbeef10000000000000000000000000000000",
        "withdrawal_percent": 50.0
      },
      {
        "receival_address": "0xdeadbeef20000000000000000000000000000000",
        "withdrawal_percent": 50.0
      }
    ]
  }
}
```

If you would like to supply custom settings for an account, or if your project does not have defaults, then you will need to supply data in the creation request.

### Making the Request

To create an account based on project defaults, you should make a POST request to `"https://ethereum-processor.herokuapp.com/accounts`. You will need to

1. supply your Project Key in the headers, using the key `PROJECT_KEY`.
2. supply account parameters as valid json POSt data. Your parameters will only be accepted if you send a post request with valid JSON data and the content-type header (`application/json`).

<aside class="notice">
  If there is no project default for a field, and you attempt this request without supplying a corresponding value, an error is returned. 
</aside>

<aside class="notice">
  If you do supply a value for a field with a default, the value you supply will override the default.
</aside>

<aside class="notice">
  The deposit behavior dictionary is treated as a whole. You cannot partially override the withdrawal percentages or addresses for your recipients. You must resupply a new, self-contained list of recipients.
</aside>


## Get All Accounts

> Request:

```python
requests.get(
  "https://ethereum-processor.herokuapp.com/accounts",
  headers={
    "PROJECT_KEY": "YOUR_KEY"
  })
```

> Response:

```json
[
  {
    "public_address": "0x890961ba525a60bd23dbd61bc0fdf6faa8440cd4",
    "confirmed_balance": 0,
    "pending_balance": 0,
    "withdrawn_balance": 0
  },
  {
    "public_address": "0xbb83b9c468fa6c8c959fd4faf0f4cc89f5223083",
    "confirmed_balance": 0,
    "pending_balance": 0,
    "withdrawn_balance": 0
  },
  {
    "public_address": "0x68bc4251db49cfe754bc58afeac1ef27e8e29cd7",
    "confirmed_balance": 0,
    "pending_balance": 0,
    "withdrawn_balance": 0
  }
]
```

This endpoint returns a list of all accounts associated with a project along with three balances (`pending_balance`, `confirmed_balance`, `withdrawn_balance`).

### Understanding the Response Data

This endpoint returns your accounts, with a focus on operational rather than configuration data. All the balance fields are displayed in Wei. The meaning of each is as follows:

1. `pending_balance` corresponds to the sum of all deposits that have not yet been confirmed.
2. `confirmed_balance` corresponds to the sum of all confirmed deposits that still remains in the account. IE, that has not been withdrawn. Most projects should always show 0 for this field unless the hotwallet is being used as a store of value.
3. `withdrawn_balance` the total amount that has been passed onto recipients.

## Get One Accounts

> Request:

```python
requests.get(
  "https://ethereum-processor.herokuapp.com/accounts/<PUBLIC_ADDRESS>",
  headers={
    "PROJECT_KEY": "YOUR_KEY"
  })
```

> Response:

```json
{
  "public_address": "0x7e749281df99fc50d54d8c385b9fdde2d9f40f69",
  "confirmed_balance": 0,
  "pending_balance": 0,
  "withdrawn_balance": 0
}
```

This route allows you to query a single account. The fields displayed are exactly the same as those described in the **Get All Accounts** section above. 