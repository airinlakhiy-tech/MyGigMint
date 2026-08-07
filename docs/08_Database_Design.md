# MyGigMint – Database Design

**Document Version:** 1.0

**Document Type:** Database Design Specification

**Project:** MyGigMint

---

# 1. Purpose

This document defines the database architecture, table structure, relationships, indexes, constraints, and data integrity rules for the MyGigMint platform.

Primary Database:

- PostgreSQL

ORM:

- Laravel Eloquent ORM

---

# 2. Database Design Principles

The database shall follow:

- Third Normal Form (3NF)
- ACID Compliance
- Foreign Key Constraints
- Proper Indexing
- Soft Deletes
- UUID Support (optional)
- Timestamp Tracking

---

# 3. Core Tables

## Users

Stores all registered users.

Columns

- id
- full_name
- username
- email
- phone
- password
- role_id
- status
- profile_photo
- referral_code
- referred_by
- email_verified_at
- created_at
- updated_at

Indexes

- email
- username
- referral_code

---

## Roles

Stores RBAC roles.

Columns

- id
- name
- description

Examples

- User
- Employer
- Moderator
- Admin
- Super Admin

---

## Permissions

Stores system permissions.

Columns

- id
- permission_name
- module

---

## Role_Permissions

Many-to-many relationship.

Columns

- role_id
- permission_id

---

## User_Sessions

Stores login sessions.

Columns

- id
- user_id
- ip_address
- device
- browser
- login_at
- logout_at

---

## Password_Resets

Stores password reset requests.

Columns

- email
- token
- expires_at

---

## Notifications

Columns

- id
- user_id
- title
- message
- type
- is_read
- created_at

---

## Activity_Logs

Stores all user activities.

Columns

- id
- user_id
- action
- ip
- device
- created_at

---

# Relationships

Users

↓

Roles

↓

Permissions

Users

↓

Notifications

Users

↓

Activity Logs

Users

↓

Sessions

---

# End of Part 1
---

# Part 2 – Job Management Database

## 4. Job Categories

Stores all job categories.

### Columns

- id
- name
- slug
- description
- icon
- status
- created_at
- updated_at

Indexes

- name
- slug

---

## 5. Jobs

Stores all posted jobs.

### Columns

- id
- employer_id
- category_id
- title
- slug
- description
- instructions
- reward_amount
- available_slots
- completed_slots
- start_date
- end_date
- status
- created_at
- updated_at

Indexes

- employer_id
- category_id
- status

---

## 6. Job Applications

Stores worker applications.

### Columns

- id
- job_id
- user_id
- proof
- status
- submitted_at
- reviewed_at
- created_at
- updated_at

Status

- Pending
- Approved
- Rejected

Indexes

- job_id
- user_id
- status

---

## 7. Job Proofs

Stores uploaded proof for completed jobs.

### Columns

- id
- application_id
- file_url
- proof_text
- reviewed_by
- review_note
- created_at

---

## 8. Skills

Stores platform skills.

### Columns

- id
- name
- slug
- description

---

## 9. User Skills

Many-to-many relationship.

### Columns

- id
- user_id
- skill_id
- experience_level

Experience Level

- Beginner
- Intermediate
- Advanced
- Expert

---

## 10. Favorites

Stores bookmarked jobs.

### Columns

- id
- user_id
- job_id
- created_at

---

## 11. Reviews

Stores employer and worker reviews.

### Columns

- id
- reviewer_id
- target_user_id
- job_id
- rating
- review
- created_at

Rating

- 1
- 2
- 3
- 4
- 5

---

# Relationships

Users
↓
Jobs

Jobs
↓
Applications

Applications
↓
Proofs

Users
↓
Skills

Users
↓
Favorites

Users
↓
Reviews

Jobs
↓
Reviews

---

# End of Part 2
---

# Part 3 – Wallet & Financial Database

## 12. Wallets

Stores each user's wallet.

### Columns

- id
- user_id
- available_balance
- pending_balance
- total_earned
- total_withdrawn
- currency
- status
- created_at
- updated_at

Indexes

- user_id

---

## 13. Wallet Transactions

Stores all wallet transactions.

### Columns

- id
- wallet_id
- user_id
- transaction_type
- amount
- balance_before
- balance_after
- reference_number
- description
- status
- created_at

Transaction Types

- Deposit
- Withdrawal
- Job Reward
- Referral Bonus
- Admin Adjustment
- Refund

Indexes

- wallet_id
- user_id
- transaction_type

---

## 14. Deposits

Stores user deposits.

### Columns

- id
- user_id
- payment_method_id
- amount
- transaction_id
- payment_proof
- status
- verified_by
- verified_at
- created_at

Status

- Pending
- Approved
- Rejected

---

## 15. Withdrawals

Stores withdrawal requests.

### Columns

- id
- user_id
- payment_method_id
- amount
- fee
- payable_amount
- account_number
- transaction_id
- status
- approved_by
- approved_at
- created_at

Indexes

- user_id
- status

---

## 16. Payment Methods

Stores supported payment methods.

### Columns

- id
- name
- type
- minimum_amount
- maximum_amount
- processing_fee
- status

Examples

- bKash
- Nagad
- Rocket
- Bank Transfer
- USDT

---

## 17. Referral System

Stores referral relationships.

### Columns

- id
- referrer_id
- referred_user_id
- referral_level
- referral_bonus
- created_at

---

## 18. Referral Earnings

Stores referral commission history.

### Columns

- id
- referral_id
- earner_id
- amount
- source
- created_at

---

## 19. Bonus History

Stores bonus rewards.

### Columns

- id
- user_id
- bonus_type
- amount
- description
- created_at

Bonus Types

- Daily Bonus
- Weekly Bonus
- Monthly Bonus
- Promotional Bonus
- Achievement Bonus

---

# Relationships

Users
↓
Wallets

Wallets
↓
Wallet Transactions

Users
↓
Deposits

Users
↓
Withdrawals

Users
↓
Referrals

Referrals
↓
Referral Earnings

Users
↓
Bonus History

---

# End of Part 3
