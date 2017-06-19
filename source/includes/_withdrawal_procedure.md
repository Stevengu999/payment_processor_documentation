# Payment Handling Procedure

## Deposits

### Detection

When a deposit is first detected, we record it as a Pending Deposit. Typically, it takes 1-3 minutes to detect a payment after it is made.

### Confirmation

When the number of blocks after the block containing the transaction equals the confirmation level of the account to which the deposit was made, the deposit is considered as Confirmed. At this point, we make a callback to your servers and process the withdrawals. Typically, confirmation will occur 5-10 minutes from when a payment is made.

### Voiding

If a deposit does not reach the confirmation level after 24 hours from its time of detection, the deposit is voided. This should almost never occur. It happens when a transaction appears in an UNCLEd block.

## Withdrawals

When a deposit is confirmed, we create withdrawals to distribute the funds to the recipients configured. Each withdrawal amount is calculated based on the recipient's withdrawal percentage. 

### Fees

Before processing any withdrawals, we deduct our processing fee. This fee is equal to the Ethereum transaction fee required for us to withdraw the fee from the account. IE, if the Ethereum fee for a withdrawal from the account to our account is 100000 Wei, we will charge 100000 Wei to process the deposit. This means a total of 200000 is deducted from the account (our fee and the amount to pay for the transaction).

Following this, we transfer funds to the recipients specified on the account. The actual amount transferred to each recipient is the percent of the deposit owed to them MINUS the Ethereum transaction fee for the withdrawal.

### Confirmation and Voiding

We monitor all withdrawals to ensure they are confirmed based on the same procedure above. If the withdrawals fail, they are automatically retried until they succeed. 






