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
