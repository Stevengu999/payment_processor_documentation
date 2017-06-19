# Callbacks

When an account receives a deposit, a callback will be made to the URL in the Account's `callback` field with the data you need to associate the deposit with an account.


## Callback Flow

### Method

The callback will be made as a POST request, with the data sent as JSON data. Your server must be able to accept POST requests with content-type `application/json`

### Required Response

If you server receives a callback, and processes it successfully, you must respond with with the following json: `{"status": "ok"}`. This is the OK response. If you do not respond with this, the callback will be considered a failure and the retry procedure will be enacted.

### Retry procedure

If we cannot reach your server, or we do not receive the OK response, then we will begin retrying the callback. We will retry every hour for the first 3 days and then every 3 hours for the next 4 days. This means we will retry for a total of one week.

<aside class="notice">
  At the end of the week, we will no longer retry the callback. This means that if your server is down for the full week after a deposit, you will need to use the Account endpoints to check if any payments occured.
</aside>

## Understanding the Data

The data in the callback is designed to help you securely associate incoming payments with users. The fields represent the following:

1. `account_address` is the `public_address` of the account to which the payment is made. It will be an account that is part of your project. You should use this value to associate the payment with a user.

2. `account_secret` is the first 4 digits of the account's `secret_withdrawal_key`. You should only process a callback if these digits match the `secret_withdrawal_key` you stored when you created the account.

<aside class="notice">
  If you do not check for this match, anyone could make post requests to your callback URL and trick you into thinking deposits occured.
</aside>

3. The outer occurence of `amount_in_wei` is the amount that was paid to the account.

4. `tx_hash` is the TX hash of the transaction in which the payment was made. It represents a payment from your user's Ethereum account into the hotwallet account.

5. The `withdrawals` field contains an array of all of the payments that were made by our API to distribute the funds to the recipients specified in deposit behavior. Each withdrawal has the address of the recipient, the amount, and the TX hash of the withdrawal on the Ethereum chain.

> The data sent with a callback

```json
{
  "account_address": "0xdeadbeefefbccee2a3a63a10b9d891f8060bbd1b",
  "account_secret": "fcadb",
  "amount_in_wei": 100000000000000000,
  "tx_hash": "0x57defbf2f494b8873bbddba0e0e0139db14def4a7e5d4c3e65d8ed2a6d29b364",
  "withdrawals": [
    {
      "recipient_address": "0xdeadbeeff9ccefc81badf3fd362bedd094c1881c",
      "tx_hash": "0aaaaaaacbe9d2cafaa28d6513944d2d24dab2c68d3a6730bbc4abd54e62aad297",
      "amount_in_wei": 100000000000000000
    }
  ]
}
```





