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

Two types of fees affect the actual amounts transferred to recipients. 

**Processing Fees**:

  Before processing any transfers to recipients, we deduct a processing fee. This fee is deducted exactly once per incoming payment. The amount charged is equal to the Ethereum Transaction Fee (ETF) required for us to make a transfer from the account to our receiving address. IE, if the ETF for a transfer from the hotwallet account to us is 100000 wei, we will a charge a processing fee of 100000 Wei. This means an effective total of 200000 is deducted from the account.

  This means that we charge a nominal, flat fee per transaction which is benchmarked to the cost of transacting on the Ethereum chain.

**Ethereum Transaction Fees**:

  Following this, we transfer funds to the recipients configured on the account. The actual amount transferred to each recipient is the percent of the deposit owed to them MINUS the Ethereum transaction fee for the transaction to them. 

<aside class="notice">
  Note that transfers to complex contracts (which execute code upon the receival of funds) will consume more gas and thus more Ether will be consumed in the transaction. Similarly, more recipients will consume more of the total either paid to the account during the withdrawal process.
</aside>

### Confirmation and Voiding

We monitor all withdrawals to ensure they are confirmed based on the same procedure used for deposits. If any withdrawal fails, it is automatically retried until it succeeds. 






