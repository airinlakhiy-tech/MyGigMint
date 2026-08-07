# MyGigMint – Wallet & Ledger System

**Document Version:** 1.0

**Document Type:** Wallet & Financial Ledger Specification

**Project:** MyGigMint

---

# 1. Purpose

This document defines the architecture and functional requirements for the MyGigMint Wallet and Ledger System.

The system shall provide a secure, auditable, consistent, and scalable mechanism for managing:

- User Wallets
- Available Balance
- Pending Balance
- Locked Balance
- Credits
- Debits
- Financial Transactions
- Double-Entry Ledger
- Wallet History
- Adjustments
- Reversals
- Reconciliation

---

# 2. Wallet Objectives

The Wallet System shall:

- Maintain accurate balances
- Prevent unauthorized balance changes
- Track every financial movement
- Support deposits
- Support withdrawals
- Support earnings
- Support refunds
- Support fees
- Support promotional credits
- Support financial adjustments
- Maintain immutable financial history

---

# 3. Wallet Architecture

The wallet system shall be separated from the frontend.

```text
User
 │
 ▼
Frontend
 │
 ▼
Wallet API
 │
 ▼
Wallet Service
 │
 ├───────────────┐
 ▼               ▼
Wallet        Ledger Service
 │               │
 └───────┬───────┘
         ▼
      Database
         │
         ▼
   Audit Logging
```

---

# 4. Wallet Ownership

Each eligible user shall have a wallet.

Relationship:

```text
User
  │
  └── 1 Wallet
        │
        ├── Balance
        ├── Ledger Entries
        └── Transactions
```

A wallet shall belong to exactly one user.

---

# 5. Wallet Status

Wallets may have the following statuses:

```text
Active
Restricted
Frozen
Suspended
Closed
```

## Active

Normal wallet operations are permitted.

## Restricted

Some operations may be blocked.

## Frozen

Financial activity is temporarily blocked.

## Suspended

Wallet access is disabled due to administrative or security reasons.

## Closed

Wallet is permanently closed according to applicable business and legal rules.

---

# 6. Balance Types

The wallet shall maintain multiple balance states.

## Available Balance

Funds currently available for spending or withdrawal.

## Pending Balance

Funds awaiting settlement or confirmation.

## Locked Balance

Funds temporarily unavailable because of:

- Withdrawal Processing
- Job Escrow
- Dispute
- Fraud Review
- Administrative Hold

## Total Balance

```text
Total Balance =
Available Balance
+ Pending Balance
+ Locked Balance
```

The exact accounting relationship shall be enforced by the ledger model.

---

# 7. Wallet Currency

The initial wallet currency shall be:

```text
BDT
```

Future versions may support multiple currencies.

Each wallet shall have an explicitly defined currency.

Cross-currency operations shall never occur without an approved exchange-rate mechanism.

---

# 8. Monetary Precision

Financial amounts shall not rely on binary floating-point arithmetic.

The system should use:

- Integer minor units, or
- A fixed-precision decimal type

Example:

```text
100.50 BDT
```

may be stored as:

```text
10050 minor units
```

The chosen implementation shall be consistent throughout the platform.

---

# 9. Wallet Account

Each wallet shall have a unique identifier.

Example:

```text
WAL-2026-000001
```

Wallet record may contain:

- Wallet ID
- User ID
- Currency
- Status
- Created At
- Updated At
- Version

---

# 10. Ledger Architecture

The Ledger shall be the authoritative record of financial movements.

Wallet balances shall never be changed without corresponding ledger entries.

Architecture:

```text
Financial Event
      │
      ▼
Transaction
      │
      ▼
Ledger Entries
      │
      ▼
Balance Update
```

---

# 11. Double-Entry Accounting

The system shall use double-entry accounting principles for financial movements.

Every completed financial transaction shall have balanced debit and credit entries.

Rule:

```text
Total Debits = Total Credits
```

Example:

```text
Deposit of 1000 BDT

Debit:
Payment Clearing Account
+1000

Credit:
User Wallet Account
+1000
```

---

# 12. Ledger Accounts

The platform may maintain multiple internal ledger accounts.

Examples:

```text
Payment Clearing Account
Platform Revenue Account
Fee Account
User Wallet Account
Refund Account
Escrow Account
Promotional Credit Account
Suspense Account
```

Account structure shall be configurable.

---

# 13. Ledger Entry

Each ledger entry shall contain:

- Ledger Entry ID
- Transaction ID
- Account ID
- Debit Amount
- Credit Amount
- Currency
- Description
- Reference
- Created At

Example:

```text
Ledger Entry

ID: LED-000001
Transaction: TXN-000001
Account: USER-WALLET-001
Debit: 0
Credit: 1000 BDT
```

---

# 14. Ledger Immutability

Posted ledger entries shall not be edited or deleted.

If a correction is required:

```text
Original Entry
      ↓
Reversal Entry
      ↓
Corrective Entry
```

This preserves the complete financial history.

---

# 15. Transaction vs Ledger

The system shall distinguish between:

## Transaction

Represents a business-level financial event.

Example:

```text
Withdrawal Request
```

## Ledger Entry

Represents the accounting movement caused by that event.

Example:

```text
Debit User Wallet
Credit Withdrawal Clearing Account
```

One transaction may generate multiple ledger entries.

---

# 16. Credit Operations

Wallet credits may originate from:

- Deposit
- Job Earnings
- Refund
- Promotional Credit
- Referral Reward
- Approved Adjustment

Every credit shall have a traceable source.

Example:

```text
Job Completed
      ↓
Earning Verified
      ↓
Transaction Created
      ↓
Ledger Credit
      ↓
Wallet Available Balance
```

---

# 17. Debit Operations

Wallet debits may originate from:

- Withdrawal
- Purchase
- Service Fee
- Refund Reversal
- Approved Adjustment

Debits shall only be processed after authorization and balance validation.

---

# 18. Wallet Balance Updates

Balance updates shall be atomic.

Example:

```text
BEGIN TRANSACTION

Validate Wallet
Validate Balance
Create Financial Transaction
Create Ledger Entries
Update Wallet State
Create Audit Record

COMMIT
```

If any critical step fails:

```text
ROLLBACK
```

---

# 19. Negative Balance Protection

The system shall prevent negative wallet balances unless explicitly supported by a documented credit/debit model.

Before debit:

```text
Available Balance >= Required Amount
```

If the condition fails:

```text
Reject Transaction
```

---

# 20. Concurrent Transaction Protection

The system shall protect against race conditions.

Example:

```text
User Balance = 1000 BDT

Request A → Withdraw 800
Request B → Withdraw 800
```

Both requests must not succeed.

The system shall use appropriate database locking, atomic updates, or optimistic concurrency controls.

---

# 21. Wallet Versioning

Wallet records may use a version field for concurrency control.

Example:

```text
wallet_version = 42
```

When an update occurs:

```text
Version 42
   ↓
Update
   ↓
Version 43
```

Unexpected version changes shall cause the operation to retry or fail safely.

---

# 22. Wallet Transaction History

Users shall be able to view wallet history.

Each record may show:

- Transaction ID
- Transaction Type
- Amount
- Fee
- Status
- Date
- Description
- Reference

Examples:

```text
+1000 BDT   Deposit
+500 BDT    Job Earnings
-200 BDT    Withdrawal
-20 BDT     Withdrawal Fee
+300 BDT    Refund
```

---

# 23. Transaction Status

Wallet transactions shall support statuses such as:

```text
Pending
Processing
Completed
Failed
Cancelled
Reversed
Refunded
```

Status transitions shall follow controlled business rules.

---

# 24. Pending Balance

Funds shall remain in Pending Balance when they cannot yet be used.

Examples:

- Payment Awaiting Confirmation
- Job Completion Awaiting Verification
- Promotional Reward Pending
- Settlement Pending

After successful settlement:

```text
Pending
   ↓
Available
```

---

# 25. Locked Balance

Funds may be moved to Locked Balance when temporarily unavailable.

Examples:

```text
Withdrawal Request
      ↓
Available Balance
      ↓
Locked Balance
      ↓
Withdrawal Completed
```

If the withdrawal fails:

```text
Locked Balance
      ↓
Available Balance
```

---

# 26. Wallet Freeze

An authorized administrator or automated security system may freeze a wallet.

Possible reasons:

- Fraud Investigation
- Account Compromise
- Chargeback
- Regulatory Requirement
- Security Incident

Wallet freeze actions shall be fully audited.

---

# 27. Wallet Unfreeze

A frozen wallet may be restored after:

- Investigation
- Identity Verification
- Risk Review
- Administrative Approval

The system shall record:

- Who unfroze the wallet
- Reason
- Timestamp
- Related case/reference

---

# 28. Wallet Adjustment

Authorized administrators may create adjustments only through a controlled process.

Example:

```text
Adjustment Request
      ↓
Reason Required
      ↓
Authorization
      ↓
Ledger Entry
      ↓
Wallet Update
      ↓
Audit Log
```

Direct database balance modification shall not be permitted.

---

# 29. Wallet Reversal

A transaction reversal shall create compensating ledger entries.

Example:

```text
Original:
+1000 BDT

Reversal:
-1000 BDT
```

The original transaction shall remain preserved.

---

# 30. Wallet Reconciliation

The system shall periodically verify:

```text
Wallet Balance
        =
Ledger-Derived Balance
```

Any difference shall create a reconciliation exception.

---

# 31. Reconciliation Exception

Examples:

- Ledger/Wallet Difference
- Missing Ledger Entry
- Duplicate Ledger Entry
- Incorrect Balance
- Failed Transaction Update

Exceptions shall be:

- Logged
- Investigated
- Resolved
- Audited

---

# 32. Wallet Security

The wallet system shall implement:

- Authentication
- Authorization
- Transaction Validation
- Rate Limiting
- Idempotency
- Audit Logging
- Fraud Detection
- Concurrency Protection

Sensitive wallet operations may require MFA or step-up authentication.

---

# 33. Wallet API

Example endpoints:

```text
GET    /api/wallet
GET    /api/wallet/balance
GET    /api/wallet/transactions
GET    /api/wallet/transactions/{id}
POST   /api/wallet/deposit
POST   /api/wallet/withdraw
POST   /api/wallet/transfer
```

All endpoints shall be protected by authentication and authorization.

---

# 34. Wallet API Response

Example:

```json
{
  "wallet_id": "WAL-2026-000001",
  "currency": "BDT",
  "available_balance": 5000,
  "pending_balance": 500,
  "locked_balance": 200,
  "total_balance": 5700
}
```

Sensitive internal accounting information shall not be exposed unnecessarily.

---

# 35. Wallet Transaction Idempotency

Critical wallet operations shall require idempotency.

Examples:

- Deposit
- Withdrawal
- Transfer
- Refund
- Adjustment

Example:

```text
Idempotency-Key:
wallet-operation-unique-key
```

Repeated requests using the same valid key shall not create duplicate financial movements.

---

# 36. Wallet Audit Trail

The system shall record:

- Wallet Created
- Deposit
- Credit
- Debit
- Withdrawal
- Freeze
- Unfreeze
- Adjustment
- Reversal
- Refund
- Administrative Action

Audit records shall be protected against unauthorized modification.

---

# End of Part 1
---
# Part 2 – Wallet Operations, Escrow, Fees, Rewards & Accounting Rules

# 37. Wallet Deposit Flow

Wallet deposit shall follow a controlled financial workflow.

```text
User
 │
 ▼
Enter Amount
 │
 ▼
Select Payment Method
 │
 ▼
Validate Amount & Limits
 │
 ▼
Create Deposit Transaction
 │
 ▼
Payment Gateway
 │
 ▼
Payment Confirmation
 │
 ▼
Ledger Posting
 │
 ▼
Pending / Available Balance
 │
 ▼
Notification
```

The wallet shall never be credited solely because the frontend reports a successful payment.

---

# 38. Deposit Settlement

A successful external payment may initially enter a settlement state.

```text
Payment Successful
        ↓
Verification
        ↓
Settlement
        ↓
Ledger Credit
        ↓
Available Balance
```

The exact settlement process shall depend on the payment provider and business rules.

---

# 39. Job Earnings

When a user successfully completes an eligible job:

```text
Job Completed
      ↓
Submission Verified
      ↓
Earning Calculated
      ↓
Transaction Created
      ↓
Ledger Entry
      ↓
Pending Balance
      ↓
Settlement Period
      ↓
Available Balance
```

The platform shall not credit earnings before the required verification conditions are satisfied.

---

# 40. Earning Calculation

Earnings shall be calculated server-side.

Example:

```text
Job Reward = 100 BDT
Platform Fee = 10 BDT
User Earning = 90 BDT
```

The exact fee model shall be configurable.

The calculation shall be recorded for audit purposes.

---

# 41. Earning Status

Job-related earnings may use the following statuses:

```text
Pending
Verified
Available
Rejected
Reversed
```

Example:

```text
Job Submitted
     ↓
Pending
     ↓
Verified
     ↓
Available
```

---

# 42. Escrow Architecture

Where the business model requires it, MyGigMint may use an escrow mechanism.

Example:

```text
Employer Wallet
      ↓
Escrow
      ↓
Job Completed
      ↓
Submission Approved
      ↓
User Wallet
```

Escrowed funds shall not be available for withdrawal until release conditions are satisfied.

---

# 43. Escrow Account

Escrow funds shall be represented separately from normal user wallet balances.

Example:

```text
Employer Wallet
      ↓
Escrow Account
      ↓
Job Settlement
      ↓
Worker Wallet
```

Escrow records shall be linked to:

- Job ID
- Employer ID
- Worker ID
- Transaction ID
- Amount
- Status

---

# 44. Escrow States

Escrow may have the following states:

```text
Created
Funded
Locked
Released
Partially Released
Refunded
Disputed
Cancelled
```

Every state transition shall be validated.

---

# 45. Escrow Release

Escrow may be released when:

- Job is successfully completed
- Submission is approved
- Dispute period expires
- Authorized administrator approves release

Release shall create corresponding ledger entries.

---

# 46. Escrow Refund

If a job is cancelled or a valid dispute is resolved in favor of the employer:

```text
Escrow
   ↓
Refund
   ↓
Employer Wallet
```

The original escrow transaction shall remain preserved.

---

# 47. Platform Fees

The platform may charge fees for:

- Job Transactions
- Premium Purchases
- Withdrawals
- Services
- Payment Processing

Fees shall be configurable.

---

# 48. Fee Calculation

Example:

```text
Gross Amount = 1000 BDT
Platform Fee = 10%
Fee = 100 BDT
Net Amount = 900 BDT
```

The system shall calculate fees on the backend.

---

# 49. Fee Ledger Entries

Fees shall be recorded separately where accounting requirements require it.

Example:

```text
User/Employer Account
        ↓
1000 BDT Debit

Platform Revenue
        ↓
100 BDT Credit

Worker/Recipient
        ↓
900 BDT Credit
```

The exact accounting entries shall be defined according to the final business model and accounting policy.

---

# 50. Withdrawal Flow

Withdrawal shall follow:

```text
User
 │
 ▼
Enter Amount
 │
 ▼
Select Payment Method
 │
 ▼
Validate Balance
 │
 ▼
Calculate Fee
 │
 ▼
Create Withdrawal
 │
 ▼
Lock Funds
 │
 ▼
Risk Check
 │
 ▼
Approval
 │
 ▼
Payment Provider
 │
 ▼
Provider Confirmation
 │
 ▼
Ledger Posting
 │
 ▼
Complete
```

---

# 51. Withdrawal Lock

When a withdrawal is created:

```text
Available Balance
       ↓
Locked Balance
```

The funds shall remain locked until the withdrawal is:

- Completed
- Failed
- Cancelled
- Rejected

---

# 52. Withdrawal Completion

When the provider confirms successful withdrawal:

```text
Locked Balance
       ↓
Withdrawal Completed
       ↓
Ledger Finalized
```

The completed transaction shall not be processed again.

---

# 53. Failed Withdrawal

If withdrawal fails:

```text
Locked Balance
       ↓
Withdrawal Failed
       ↓
Reversal Ledger Entry
       ↓
Available Balance
```

The reversal shall be linked to the original withdrawal transaction.

---

# 54. Referral Rewards

The platform may provide referral rewards.

Referral reward flow:

```text
Referral Activity
      ↓
Eligibility Check
      ↓
Fraud Check
      ↓
Reward Calculation
      ↓
Pending Reward
      ↓
Validation Period
      ↓
Available Reward
```

Referral rewards shall not become immediately withdrawable unless the applicable business rules allow it.

---

# 55. Referral Reward Ledger

Each referral reward shall have:

- Reward ID
- Referrer ID
- Referred User ID
- Trigger Event
- Amount
- Status
- Transaction ID
- Created At

---

# 56. Promotional Credits

The platform may issue promotional credits.

Examples:

- Welcome Bonus
- Campaign Reward
- Promotional Credit
- Loyalty Reward

Promotional credits may have:

- Expiration
- Usage Restrictions
- Withdrawal Restrictions
- Minimum Activity Requirements

These rules shall be explicitly defined before implementation.

---

# 57. Promotional Credit Separation

Where necessary, promotional funds shall be tracked separately from withdrawable funds.

Example:

```text
Real Balance
+
Promotional Balance
=
Displayed Total
```

The system shall clearly distinguish which portion is actually withdrawable.

---

# 58. Internal Wallet Transfers

If user-to-user transfers are supported, transfers shall use controlled ledger operations.

Example:

```text
Sender Wallet
     │
     ▼
Transfer Transaction
     │
     ▼
Recipient Wallet
```

The operation shall be atomic.

Both sides shall succeed or both sides shall fail.

---

# 59. Transfer Validation

Before a transfer is processed, the system shall validate:

- Sender Authentication
- Sender Wallet Status
- Recipient Validity
- Sufficient Balance
- Transfer Limits
- Risk Rules
- Idempotency Key

---

# 60. Transfer Ledger Example

For a 500 BDT transfer:

```text
Sender Wallet
Debit: 500 BDT

Recipient Wallet
Credit: 500 BDT
```

If a platform transfer fee applies:

```text
Sender Wallet
Debit: 520 BDT

Recipient Wallet
Credit: 500 BDT

Platform Fee Account
Credit: 20 BDT
```

---

# 61. Transaction Fees

Every fee-bearing operation shall clearly display:

```text
Amount
+
Fee
=
Total
```

Example:

```text
Withdrawal: 1000 BDT
Fee: 20 BDT
Total Deduction: 1020 BDT
```

The user shall confirm the final amount before execution.

---

# 62. Minimum and Maximum Limits

Wallet operations shall support configurable limits.

Examples:

```text
Minimum Deposit
Maximum Deposit
Minimum Withdrawal
Maximum Withdrawal
Daily Withdrawal Limit
Daily Transfer Limit
Monthly Limits
```

Limits may depend on:

- Account Verification
- Risk Score
- User Role
- Payment Method
- Regulatory Requirements

---

# 63. Transaction Reference

Every wallet operation shall have a traceable reference.

Example:

```text
Transaction ID:
TXN-2026-00001234

Reference:
JOB-000456
```

References shall allow administrators and support teams to trace financial events.

---

# 64. Accounting Event Model

Financial events shall be represented independently from UI actions.

Example:

```text
Business Event
      ↓
Financial Transaction
      ↓
Ledger Entries
      ↓
Balance Update
      ↓
Audit Event
```

This separation improves reliability and auditability.

---

# 65. Accounting Example – Deposit

User deposits 1000 BDT.

```text
Transaction:
DEPOSIT-001

Debit:
Payment Clearing Account
1000 BDT

Credit:
User Wallet
1000 BDT
```

After settlement:

```text
User Available Balance
+1000 BDT
```

---

# 66. Accounting Example – Withdrawal

User withdraws 1000 BDT.

```text
Debit:
User Wallet
1000 BDT

Credit:
Withdrawal Clearing Account
1000 BDT
```

If a 20 BDT fee applies:

```text
Debit:
User Wallet
1020 BDT

Credit:
Withdrawal Clearing
1000 BDT

Credit:
Platform Fee Account
20 BDT
```

---

# 67. Accounting Example – Job Payment

Employer pays 1000 BDT for a job.

```text
Employer / Escrow
Debit: 1000 BDT

Escrow Account
Credit: 1000 BDT
```

After successful completion:

```text
Escrow Account
Debit: 1000 BDT

Worker Wallet
Credit: 900 BDT

Platform Revenue
Credit: 100 BDT
```

The exact accounting treatment shall be finalized according to the business model and applicable accounting requirements.

---

# 68. Accounting Example – Refund

A 500 BDT refund is issued.

```text
Refund Account
Debit: 500 BDT

User/Customer Account
Credit: 500 BDT
```

The refund shall reference the original transaction.

---

# 69. Accounting Example – Reversal

If an incorrect 500 BDT credit must be reversed:

Original:

```text
User Wallet
Credit: 500 BDT
```

Reversal:

```text
User Wallet
Debit: 500 BDT
```

The original ledger entry shall remain unchanged.

---

# 70. Accounting Example – Promotional Reward

A 200 BDT promotional reward may be posted as:

```text
Promotional Expense Account
Debit: 200 BDT

User Promotional Wallet
Credit: 200 BDT
```

Promotional balance rules shall determine whether the amount is immediately withdrawable.

---

# 71. Balance Calculation

The system shall maintain a clear relationship between ledger and wallet state.

Conceptually:

```text
Wallet Balance
=
Opening Balance
+
Credits
-
Debits
+
Adjustments
```

The ledger shall remain the authoritative source for financial history.

---

# 72. Balance Verification

The system shall periodically verify:

```text
Stored Wallet Balance
        =
Calculated Ledger Balance
```

If the values differ:

```text
Create Reconciliation Exception
        ↓
Freeze Risky Operations if Necessary
        ↓
Investigate
        ↓
Correct Through Ledger Entries
```

---

# 73. Atomic Financial Operations

A financial operation shall not partially complete.

For example:

```text
Debit Sender
+
Credit Recipient
+
Create Transaction
+
Create Audit Record
```

All required operations must either succeed together or fail together.

---

# 74. Financial Transaction Locking

Critical transactions shall use appropriate concurrency controls.

Possible mechanisms include:

- Database Row Lock
- Optimistic Locking
- Atomic SQL Update
- Serializable Transaction
- Distributed Lock where necessary

The implementation shall prevent double-spending and race conditions.

---

# 75. Wallet Event Notifications

Wallet events may trigger notifications.

Examples:

- Deposit Successful
- Withdrawal Requested
- Withdrawal Completed
- Withdrawal Failed
- Job Reward Added
- Referral Reward Added
- Refund Completed
- Wallet Frozen
- Wallet Unfrozen

Notifications shall be generated only after the financial transaction reaches the appropriate state.

---

# End of Part 2
