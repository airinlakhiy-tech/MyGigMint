# MyGigMint – Payment System

**Document Version:** 1.0

**Document Type:** Payment System Specification

**Project:** MyGigMint

---

# 1. Purpose

This document defines the payment architecture, transaction processing, deposits, withdrawals, payment gateways, refunds, payment verification, webhooks, fraud controls, and financial audit requirements for MyGigMint.

The payment system shall provide secure, reliable, auditable, and scalable financial operations.

---

# 2. Payment Objectives

The payment system shall support:

- User Deposits
- Premium Purchases
- Job Payments
- Wallet Credits
- Withdrawal Requests
- Refunds
- Payment Verification
- Transaction History
- Payment Gateway Integration
- Webhook Processing
- Fraud Detection
- Financial Audit Logging

---

# 3. Payment Architecture

The payment system shall use a centralized Payment Service.

```text
                    User
                      │
                      ▼
                MyGigMint UI
                      │
                      ▼
                Payment API
                      │
                      ▼
                Payment Service
          ┌───────────┼────────────┐
          ▼           ▼            ▼
       Wallet     Transaction   Risk Engine
          │           │            │
          └───────────┼────────────┘
                      ▼
                Payment Gateway
                      │
          ┌───────────┼────────────┐
          ▼           ▼            ▼
        bKash       Nagad       Card Provider
                      │
                      ▼
                  Webhook
                      │
                      ▼
              Payment Verification
                      │
                      ▼
                  Wallet Update
```

---

# 4. Payment Providers

The platform shall support configurable payment providers.

Potential providers:

- bKash
- Nagad
- Card Payment Processor
- Stripe (where supported)
- Bank Transfer
- Other approved providers

Payment providers shall be integrated through secure APIs.

---

# 5. Supported Currencies

The initial platform currency shall be:

```text
BDT – Bangladeshi Taka
```

Future versions may support additional currencies.

Currency handling shall use standardized currency codes.

Examples:

```text
BDT
USD
EUR
```

---

# 6. Payment Types

The system shall support:

## Deposit

User adds funds to their wallet.

## Purchase

User purchases a product, service, or premium membership.

## Job Payment

Employer funds a job-related payment.

## Withdrawal

User requests money from their available balance.

## Refund

Previously completed payment is returned according to applicable rules.

## Adjustment

Authorized administrative correction.

All financial adjustments shall be audited.

---

# 7. Transaction Lifecycle

Every transaction shall follow a defined lifecycle.

```text
Created
   ↓
Pending
   ↓
Processing
   ↓
Successful
   │
   ├── Failed
   │
   └── Cancelled
```

Possible final states:

- Successful
- Failed
- Cancelled
- Refunded
- Partially Refunded

---

# 8. Transaction States

## Created

Transaction record has been created.

## Pending

Payment has not yet been confirmed.

## Processing

Payment provider is processing the request.

## Successful

Payment has been verified successfully.

## Failed

Payment could not be completed.

## Cancelled

Transaction was cancelled before completion.

## Refunded

Funds were returned.

---

# 9. Transaction ID

Every transaction shall have a globally unique identifier.

Example:

```text
TXN-2026-00000001
```

Transaction IDs shall be:

- Unique
- Non-guessable where appropriate
- Traceable
- Immutable

---

# 10. Transaction Record

A transaction shall contain:

- Transaction ID
- User ID
- Amount
- Currency
- Transaction Type
- Payment Method
- Payment Provider
- Provider Transaction ID
- Status
- Created At
- Updated At
- Completed At
- Reference ID
- Failure Reason

Sensitive payment credentials shall not be stored unnecessarily.

---

# 11. Payment Initiation

Payment initiation flow:

```text
User
 │
 ▼
Select Payment Method
 │
 ▼
Enter Amount
 │
 ▼
Validate Request
 │
 ▼
Create Transaction
 │
 ▼
Generate Payment Request
 │
 ▼
Redirect / Payment Interface
 │
 ▼
Payment Provider
```

---

# 12. Amount Validation

The backend shall validate:

- Minimum Amount
- Maximum Amount
- Currency
- Decimal Precision
- User Balance where applicable
- Transaction Limits

The frontend shall never be trusted for financial calculations.

---

# 13. Financial Calculation

All financial calculations shall be performed server-side.

The system shall avoid floating-point errors.

Recommended approach:

```text
Store monetary values as integer minor units
```

Example:

```text
100 BDT
=
10000 minor units
```

The exact implementation shall depend on the selected currency and accounting model.

---

# 14. Payment Fees

Payment fees shall be configurable.

Possible fee types:

- Percentage Fee
- Fixed Fee
- Provider Fee
- Withdrawal Fee
- Service Fee

Example:

```text
Payment Amount = 1000 BDT
Service Fee = 20 BDT
Total = 1020 BDT
```

The final fee calculation shall always be performed server-side.

---

# 15. Payment Confirmation

A payment shall only be marked successful after server-side verification.

The platform shall not trust:

- Browser Redirect
- Frontend Success Message
- Client-Side Callback

Primary confirmation should come from a trusted provider response or webhook, with provider-specific verification.

---

# 16. Webhook Processing

Payment providers may send asynchronous webhooks.

Webhook flow:

```text
Payment Provider
       │
       ▼
Webhook Endpoint
       │
       ▼
Signature Verification
       │
       ▼
Event Validation
       │
       ▼
Idempotency Check
       │
       ▼
Update Transaction
       │
       ▼
Update Wallet / Order
       │
       ▼
Audit Log
```

---

# 17. Webhook Security

Webhook endpoints shall implement:

- HTTPS
- Signature Verification
- Request Validation
- Replay Protection where supported
- Idempotency
- Rate Limiting
- Audit Logging

Invalid webhook requests shall be rejected.

---

# 18. Idempotency

Payment creation and webhook processing shall support idempotency.

Example:

```text
Idempotency-Key:
pay_01_example_unique_key
```

Repeated requests shall not create duplicate financial transactions.

---

# 19. Payment Failure Handling

If a payment fails:

- Transaction shall be marked Failed
- Wallet shall not be credited
- User shall receive appropriate notification
- Failure reason shall be recorded safely
- Provider reference shall be retained when appropriate

---

# 20. Payment Timeout

Pending payments shall have a configurable timeout.

Example:

```text
Created
   ↓
Pending
   ↓
Timeout
   ↓
Expired
```

Expired transactions shall not automatically be treated as successful.

---

# 21. Payment Notifications

Users shall receive notifications for:

- Payment Initiated
- Payment Successful
- Payment Failed
- Payment Refunded
- Withdrawal Requested
- Withdrawal Approved
- Withdrawal Failed

Notifications shall not expose sensitive credentials.

---

# 22. Payment Audit Trail

Every important financial action shall be auditable.

Audit events:

- Payment Created
- Payment Processing
- Payment Successful
- Payment Failed
- Payment Refunded
- Withdrawal Requested
- Withdrawal Approved
- Withdrawal Rejected
- Manual Adjustment

Audit records shall include:

- Actor
- Transaction ID
- Action
- Timestamp
- Request ID
- Result

---

# End of Part 1
---

# Part 2 – Wallet, Ledger, Deposits, Withdrawals & Reconciliation

# 23. Wallet Architecture

MyGigMint shall provide a secure wallet system for managing user financial balances.

Each eligible user shall have a wallet associated with their account.

The wallet shall maintain:

- Available Balance
- Pending Balance
- Locked Balance
- Total Balance
- Currency
- Wallet Status

---

# 24. Wallet Balance

The wallet shall distinguish between different balance states.

## Available Balance

Funds that can currently be used or withdrawn.

## Pending Balance

Funds awaiting confirmation or settlement.

## Locked Balance

Funds temporarily restricted because of:

- Job Processing
- Fraud Review
- Dispute
- Withdrawal Processing
- Administrative Hold

## Total Balance

```text
Total Balance =
Available Balance
+ Pending Balance
+ Locked Balance
```

---

# 25. Double-Entry Ledger

Financial transactions should use a ledger-based accounting model.

Every financial movement shall create corresponding ledger entries.

Example:

```text
Payment Received

Debit:
Payment Clearing Account

Credit:
User Wallet Account
```

The ledger shall maintain a complete history of financial movements.

---

# 26. Ledger Requirements

Ledger entries shall include:

- Ledger ID
- Transaction ID
- Account ID
- Debit
- Credit
- Currency
- Balance Before
- Balance After
- Description
- Created At

Ledger records shall be immutable after posting.

Corrections shall be performed through new adjustment entries rather than modifying historical records.

---

# 27. Wallet Transaction Flow

```text
Payment
   ↓
Payment Verification
   ↓
Transaction Created
   ↓
Ledger Entry
   ↓
Wallet Balance Updated
   ↓
Transaction Completed
   ↓
Notification
```

All wallet updates shall occur within appropriate database transactions to maintain consistency.

---

# 28. Deposit Flow

Deposit process:

```text
User
 │
 ▼
Enter Deposit Amount
 │
 ▼
Select Payment Method
 │
 ▼
Create Transaction
 │
 ▼
Payment Provider
 │
 ▼
Payment Completed
 │
 ▼
Webhook / Verification
 │
 ▼
Ledger Entry
 │
 ▼
Wallet Credit
 │
 ▼
Notification
```

Wallet funds shall only be credited after successful server-side verification.

---

# 29. Deposit Limits

The system shall support configurable deposit limits.

Possible limits:

- Minimum Deposit
- Maximum Deposit Per Transaction
- Daily Deposit Limit
- Monthly Deposit Limit

Limits may vary according to:

- User Verification Level
- Risk Score
- Payment Method
- Account Status
- Regulatory Requirements

---

# 30. Withdrawal Flow

Withdrawal process:

```text
User
 │
 ▼
Enter Withdrawal Amount
 │
 ▼
Select Withdrawal Method
 │
 ▼
Validate Balance
 │
 ▼
Risk Checks
 │
 ▼
Create Withdrawal Request
 │
 ▼
Lock Funds
 │
 ▼
Review / Approval
 │
 ▼
Payment Provider
 │
 ▼
Provider Confirmation
 │
 ▼
Complete Withdrawal
 │
 ▼
Ledger Entry
 │
 ▼
Notification
```

---

# 31. Withdrawal Validation

Before creating a withdrawal, the system shall verify:

- User Account Status
- Available Balance
- Minimum Withdrawal Amount
- Maximum Withdrawal Amount
- Daily Limit
- Payment Method
- Identity Verification Status where applicable
- Fraud Risk
- Pending Withdrawals

---

# 32. Withdrawal Fees

Withdrawal fees shall be configurable.

Example:

```text
Withdrawal Amount = 1000 BDT
Withdrawal Fee = 20 BDT
User Receives = 980 BDT
```

The system shall clearly display fees before confirmation.

---

# 33. Withdrawal Approval

Withdrawals may use automated or manual approval.

Low-risk transactions:

```text
Request
  ↓
Automatic Risk Check
  ↓
Approved
```

High-risk transactions:

```text
Request
  ↓
Risk Check
  ↓
Manual Review
  ↓
Approved / Rejected
```

---

# 34. Withdrawal Rejection

A withdrawal may be rejected because of:

- Insufficient Balance
- Failed Verification
- Fraud Risk
- Invalid Payment Details
- Account Restriction
- Provider Restrictions

Rejected withdrawals shall release any locked funds according to the transaction rules.

---

# 35. Withdrawal Failure

If the payment provider fails to process a withdrawal:

```text
Withdrawal Processing
        ↓
Provider Failure
        ↓
Mark Failed
        ↓
Release Locked Funds
        ↓
Create Reversal Ledger Entry
        ↓
Notify User
```

The original transaction record shall remain unchanged for auditability.

---

# 36. Payment Method Management

Users may manage supported payment methods.

Examples:

- Mobile Wallet
- Bank Account
- Card
- Other Approved Methods

Sensitive payment credentials shall be tokenized or stored by the payment provider whenever possible.

---

# 37. Payment Method Verification

Before using a payment method for sensitive operations, the system may require verification.

Verification methods may include:

- OTP
- Provider Verification
- Micro-Verification
- Identity Verification

---

# 38. Payment Gateway Integration

Payment gateways shall be integrated through a standardized internal interface.

Example:

```text
Payment Service
      │
      ▼
Payment Gateway Interface
      │
 ┌────┼───────────┐
 ▼    ▼           ▼
bKash Nagad    Card Provider
```

This architecture allows additional providers to be added without redesigning the entire payment system.

---

# 39. Gateway Adapter

Each payment provider shall have an adapter responsible for:

- Create Payment
- Verify Payment
- Process Webhook
- Refund Payment
- Check Payment Status
- Handle Provider Errors

Example interface:

```text
PaymentGatewayInterface

createPayment()
verifyPayment()
refundPayment()
getPaymentStatus()
handleWebhook()
```

---

# 40. bKash Integration

Where supported and approved for the platform's business model, bKash shall be integrated through its official merchant/payment APIs.

The integration shall support:

- Payment Creation
- Payment Execution
- Payment Verification
- Callback/Webhook Handling
- Transaction Status
- Error Handling

Credentials shall be stored securely and never committed to source control.

---

# 41. Nagad Integration

Where supported and approved, Nagad shall be integrated through its official merchant/payment APIs.

The integration shall support:

- Payment Initialization
- Payment Processing
- Payment Verification
- Callback Handling
- Transaction Status
- Error Handling

Provider-specific requirements shall be documented separately during implementation.

---

# 42. Provider Abstraction

The application shall not tightly couple business logic to one payment provider.

Example:

```text
Business Logic
      ↓
Payment Service
      ↓
Gateway Interface
      ↓
Provider Adapter
      ↓
External Provider
```

This allows future providers to be added with minimal changes.

---

# 43. Payment Reconciliation

The system shall periodically reconcile internal transactions with payment provider records.

Reconciliation shall compare:

- Internal Transaction ID
- Provider Transaction ID
- Amount
- Currency
- Status
- Timestamp

---

# 44. Reconciliation Process

```text
Internal Transactions
        │
        ▼
Provider Transactions
        │
        ▼
Compare Records
        │
   ┌────┴────┐
   ▼         ▼
Matched   Mismatch
   │         │
   ▼         ▼
Confirm    Review
           │
           ▼
      Reconciliation
        Resolution
```

---

# 45. Reconciliation Exceptions

Potential mismatches include:

- Missing Provider Transaction
- Missing Internal Transaction
- Amount Mismatch
- Status Mismatch
- Duplicate Transaction
- Delayed Webhook

Exceptions shall be recorded and investigated.

---

# 46. Refund Processing

Refunds shall be handled through a controlled workflow.

```text
Refund Request
      ↓
Eligibility Check
      ↓
Approval
      ↓
Payment Provider
      ↓
Provider Confirmation
      ↓
Ledger Adjustment
      ↓
Notification
```

Refunds shall never directly modify historical ledger records.

---

# 47. Partial Refund

The system may support partial refunds.

Example:

```text
Original Payment = 1000 BDT
Refund = 300 BDT
Remaining = 700 BDT
```

Each refund shall have its own transaction/reference identifier.

---

# 48. Financial Reconciliation Reports

Administrators shall have access to reports showing:

- Total Deposits
- Total Withdrawals
- Successful Payments
- Failed Payments
- Refunds
- Fees
- Pending Transactions
- Reconciliation Exceptions

Financial reports shall be access-controlled and audited.

---

# 49. Financial Data Integrity

The payment system shall ensure:

- No Negative Balance Unless Explicitly Supported
- No Duplicate Credits
- No Duplicate Withdrawals
- Immutable Ledger History
- Atomic Balance Updates
- Unique Transaction IDs
- Idempotent Payment Operations

---

# 50. Payment Database Transactions

Critical wallet operations shall use database transactions.

Example:

```text
BEGIN TRANSACTION

Create Ledger Entry
Update Wallet Balance
Update Payment Status
Create Audit Record

COMMIT
```

If any critical operation fails:

```text
ROLLBACK
```

This prevents inconsistent financial states.

---

# End of Part 2
