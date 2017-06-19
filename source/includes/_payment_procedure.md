# Payment Handling Procedure

## Deposits

### Detection

Our system continuously scans the Ethereum chain, looking for new deposits into the accounts created through the system. When a payment to an account appears in a block, we record it as a 'Pending' Deposit. Typically, it takes 1-3 minutes to detect a payment after it is made. At this point, no callback is made. The block number or 'block height' is recorded.

### Confirmation

When the number of blocks created after the one containing the transaction equal the confirmation level of the account, the payment becomes 'Confirmed'. At this point, we make a callback to your servers and process withdrawals to distribute the funds (see below). Typically, confirmation will occur 5-10 minutes from when a payment is made.

### Voiding

If a payment does not reach the confirmation level by 24 hours after it was detected, it is voided. This should almost never occur. It happens when a transaction appears in an UNCLEd block or when there is some other divergence in the chain.

## Withdrawals

When a deposit is confirmed, we trigger withdrawals to distribute the funds to the recipients configured on the account. Each withdrawal amount is calculated based on the recipient's withdrawal percentage. 

### Fees

Two types of fees affect the final amounts transferred to your configured recipients. 

**Processing Fees**:

  Before processing any transfers to recipients, we deduct a processing fee. This fee is deducted exactly once per incoming payment. The amount charged is equal to the gas required to make a transfer from the hot wallet account to our receiving address TIMES the gas price (the Ethereum Transaction Fee). This means that the amount substracted from the account before distribution will be 2 TIMES the Ethereum Transaction Fee. One half being our fee and one half paying for the transfer of our fee to us.

  This fee structure has been carefully designed. It means that we charge a nominal, flat fee per incoming transaction which is benchmarked to the cost of transacting on the Ethereum chain. The cost you will pay for this API is proportional to the activity occuring on the chain.

**Withdrawal Fee**:

  After transferring our fee, we transfer funds to the recipients configured on the account. The actual amount transferred to each recipient is the amount owed to them MINUS the Ethereum Transaction Fee for the transaction to pay them.

**Net Result**:

  If you have an account configured to distribute incoming payments to a single recipient, the recipient will receive the amount of the deposit MINUS 3 TIMES the Ethereum Transaction Fee. Of the amount subtracted, 1 parts will go to Zerion and two parts will be used to pay transaction costs. 

<aside class="notice">
  Transfers to contracts (which execute code upon the receival of funds) will consume more gas and thus more Ether will be spent on withdrawal transaction with contracts as recipients. The more complex the code run, the more gas used.
</aside>
<aside class="notice">
  Accounts with multiple recipients will result in one transaction being made per recipient. This will consume more gas, meaning that a smaller relative percentage of the incoming payment will be transferred to recipients. The change is neglegible provided the payments being received are of reasonable size. 
</aside>

### Confirmation and Voiding

We monitor all withdrawals to ensure they are confirmed based on the same procedure used for deposits. If any withdrawal fails, it is automatically retried until it succeeds. 






