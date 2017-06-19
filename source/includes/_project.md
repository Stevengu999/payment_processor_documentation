# Project

This is the base model of the API. All the accounts you create will be owned by your project. 

### Base URL
The base route related to all project actions is `https://ethereum-processor.herokuapp.com/project/defaults`. 

### Fields
Projects have 3 fields. 

Field | Format | Description
--------- | ------- | -----------
default_callback_url | String, valid URL | The URL to which a callback is made when an account receives a payment.
default_confirmation_level | Integer, minimum 1 | The number of confirmations before a received payment is treated as confirmed. 
default_deposit_behavior | JSON Dictionary | The value of this field determines how ethereum received by accounts is distributed. More on this later.

## Setting Defaults

> Request:

```python
requests.post(
  "https://ethereum-processor.herokuapp.com/project/defaults",
  json={
    "default_confirmation_level": 6,
    "default_callback": "your_app_server.com",
    "default_deposit_behavior": {
      "recipients": [
        {
          "receival_address": "0xdeadbeefD68880146dF45379661546AB4aBF6C78",
          "withdrawal_percent": 100.0
        }
      ]
    }
  },
  headers={
    "PROJECT_KEY": "YOUR_KEY"
  })
```

> Response:

```json
{
  "default_confirmation_level": 6,
  "default_callback": "your_app_server.com",
  "default_deposit_behavior": {
    "recipients": [
      {
        "receival_address": "0xdeadbeefD69595146dF45378888546AB4aBF6C78",
        "withdrawal_percent": 100.0
      }
    ]
  }
}
```

The first step in configuring the API is creating your project's default settings. These settings will be used when creating new accounts, unless custom/override values are supplied. 

### Making the Request

To set project defaults, you should make a POST request to `"https://ethereum-processor.herokuapp.com/project/defaults`. You will need to

1. supply your Project Key in the headers, using the key `PROJECT_KEY`.
2. supply data parameters encoded as valid JSON. Your parameters will only be accepted if you send a post request with valid JSON data and the content-type header (`application/json`).

### Understanding the Parameters

The `default_callback` and `default_confirmation_level` fields are fairly easy to understand. The callback will be used to make callbacks whenever an account receives a deposit and the confirmation level specifies the number of blocks required for a deposit to be considered as confirmed.

The `default_deposit_behavior` field is slightly more complex. It is a dictionary with a single key `recipients`. This key holds an array of recipients, each of which is a dictionary with a `receival_address` (a 42 character, Ethereum-address String) and a `withdrawal_percent` (a float between 0.0 and 100.0). 

<aside class="notice">
  The withdrawal percentages in the recipient list must sum to 100.
</aside>

**When a deposit is received, it will be distributed based on the receiving account's deposit_behavior**. In this case, it will be passed onto the recipients in the list provided according to the percentages provided. In the future, we may support new kinds of deposit behavior, so watch this space for more!

### Setting New Project Defaults

If you have already set some default and want to change the values, simply make a new post request with the new values specified.

<aside class="notice">
  If a default for a field has already been set, and you do not supply a value for that field in your request, the old value will be preserved. There is currently no way to unset a default once one has been set.
</aside>

## Getting Defaults

If you need to inspect your project's defaults, following the instructions below.

> Request:

```python
requests.get(
  "https://ethereum-processor.herokuapp.com/project/defaults",
  headers={
    "PROJECT_KEY": "YOUR_KEY"
  })
```

> Response:

```json
{
  "default_confirmation_level": 6,
  "default_callback": "your_app_server.com",
  "default_deposit_behavior": {
    "recipients": [
      {
        "receival_address": "0xdeadbeefD69595146dF45378888546AB4aBF6C78",
        "withdrawal_percent": 100.0
      }
    ]
  }
}
```

### Making the Request

To get a project's defaults, you should make a GET request to `"https://ethereum-processor.herokuapp.com/project/defaults`. You will need to supply your Project Key in the headers, using the key `PROJECT_KEY`.